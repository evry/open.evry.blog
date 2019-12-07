---
layout: post
author: Endre Karlson
author_hanndle: ekarlso
title: Azure AD API Authentication using GCP Cloud Endpoints
date: 2019-11-24
tag: [meta]
published: true
---

# Motivation

Some of our customers are using both Azure and GCP for their environments. In a current project we
have solutions/projects where they use GCP Cloud Functions to do calculations, data transformation
various features used by a bunch of webapps. The problem with this is that Cloud Functions 
doesn't allow for authentication without you having the identities in Google.

The solution to this is to use the Google Endpoints / ESP proxy in front of the functions you would
like Azure AD / OIDC auth for.

# Pre-requisites

* SPA app that is OIDC capable (Or use the example one.)
* API Function (Or use the example one)
* Azure AD Tenant wher you have rights to create Application Registrations
* GCP Project to deploy functions / ESP
* A clone of git@github.com:evry-ace/google-esp-azad-auth-examples.git

# Let's do it!

Ensure you have the GCP project you want to deploy to set in config and that you have selected
the correct Azure account using

*Google*

```sh
gcloud config get-value project
```

*Azure*

```sh
az account list -o tsv
```

## Azure Application Registrations

```
SERVER_NAME="test-func-gw"
CLIENT_NAME="test-spa-app"
```

### Add the api application

```sh
az ad app create --display-name $SERVER_NAME
```

*Grant admin consent*

```sh
SERVER_OBJID=$(az ad app list --query "[?displayName == '$SERVER_NAME'] | [0].objectId" -o tsv)
az ad app permission add --id $SERVER_OBJID --api 00000002-0000-0000-c000-000000000000 --api-permissions 311a71cc-e848-46a1-bdf8-97ff7156d8e6=Scope
az ad app permission admin-consent --id $SERVER_OBJID
```

Let's add a scope

```sh
az ad app show --id $SERVER_OBJID --query="oauth2Permissions" > scopes.json
```

*Add some new scopes*


```sh
vi scopes.json
```

```json
{
    "adminConsentDescription": "Allow stuff for the signed-in user.",
    "adminConsentDisplayName": "Access this new thing",
    "id": "b53a0606-5034-4815-96b2-1cd195a9facf",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow stuff on your behalf.",
    "userConsentDisplayName": "Access this new thing",
    "value": "app.scope.test"
}
```

Update the scopes for the app.

```sh
az ad app update --id $SERVER_OBJID --set oauth2Permissions=@scopes.json
```

### Add the client app

```sh
az ad app create --display-name $CLIENT_NAME \
    --reply-urls http://localhost:4200 \
    --oauth2-allow-implicit-flow=true
```

*Grant user read permission*

```sh
CLIENT_OBJID=$(az ad app list --query "[?displayName == '$CLIENT_NAME'] | [0].objectId" -o tsv)
SERVER_APPID=$(az ad app list --query "[?displayName == '$SERVER_NAME'] | [0].appId" -o tsv)

az ad app permission add \
    --id $CLIENT_OBJID --api $SERVER_APPID \
    --api-permissions <SCOPE ID FROM ABOVE>=Scope

az ad app permission admin-consent --id $CLIENT_OBJID
```

Assign the api scope to the client.

NOTE: You will need to replace the SCOPE_ID with your scope uuid that you got above.

```sh
az ad app permission add \
    --id $CLIENT_OBJID \
    --api $SERVER_OBJID \
    --api-permissions <SCOPE_ID>=Scope

az ad app permission admin-consent --id $CLIENT_OBJID
```

## Deploy the function

Set the target project

```sh
PROJECT=<replaceme>
```

Deploy the function

```sh
gcloud functions deploy \
    --region europe-west1 \
    --source api-function \ 
    --trigger-http \
    --runtime python37 \
    --entry-point get_data \
    api-function
```

Create a GCP service account for the ESP proxy

```sh
gcloud iam service-accounts create esp-func-gw
```

Assign the SA rights on the function to execute it

```sh
gcloud functions add-iam-policy-binding \
    --role=roles/cloudfunctions.invoker \
    --member="serviceAccount:esp-func-gw@$PROJECT.iam.gserviceaccount.com" \
    --region=europe-west1 \
    api-function
```

## Deploy the API GW

Now that we have the functions and other pieces ready we will deploy the CloudEndpoints 
API specification and the ESP proxy.

```
export AUD="api://YOUR_AUD"
export TENANT_ID="YOUR_TENANT"
export PROJECT="YOUR_GCP_PROJECT"

SVC_NAME="test-func-gw"

sed -e "s#_PROJECT_#$PROJECT#g" \
    -e "s#_TENANT_ID_#$TENANT_ID#g" \
    -e "s#_AUDIENCE_#$AUDIENCE#g" \
    -e "s#_SVC_#$SVC_NAME#g" \
    api-definition.tpl.yaml > $SVC_NAME.yaml

gcloud endpoints services deploy $SVC_NAME.yaml --project ${PROJECT}

gcloud beta run deploy ${SVC_NAME} \
    --image="gcr.io/endpoints-release/endpoints-runtime-serverless:1" \
    --allow-unauthenticated \
    --project=${PROJECT} \
    --platform managed \
    --service-account esp-func-gw@$PROJECT.iam.gserviceaccount.com \
    --region europe-west1 \
    --set-env-vars="ENDPOINTS_SERVICE_NAME=${SVC_NAME}.endpoints.${PROJECT}.cloud.goog, ESP_ARGS=^++^--cors_preset=basic++--cors_allow_origin=http://localhost:4200++--cors_allow_methods=GET"
```

## Configure the SPA app

Set up all your parameters to match the above.

Add the scope * api://AZAD_APP_ID/APP_SCOPE_NAME*

NOTE: That the app *AZAD_APP_ID* can be used or you can set the app uri like *test-gw-func*
under the **App Aegistration -> Expose an API -> Set** on the top of that page.

If you are using the sample app then edit the **assets/appConfig.json** accordingly.

Voila!

You should now be able to go to http://localhost:4200, be logged in and click "Fetch Data" 
# Flask test app

If you need to test if the token works you can run the example flask app and target localhost 
instead of the remote Cloud Endpoints url.

```
export AUD="api://YOUR_AUD"
export TENANT_ID="YOUR_TENANT"
python3 python-flask-api/api.py
```

You also need to configure the *apiEndpoint* key in the appConfig.json.

# Inspirational links

* https://www.frodehus.dev/add-scopes-to-azure-ad-via-azure-cli/