---
layout: post
author: Hans Kristian Flaatten
author_email: hans.flaatten@evry.com
author_hanndle: Starefossen
title: Hello Knative
date: 2019-06-30
tag: [knative, kubernetes]
published: true
---

The evolution of modern computing has taken us through smaller and smaller physical machines, to virtual machines, in the cloud, and then to containers. Kubernetes has done a tremendous job of abstracting away any notion of machines, the next step is to disconnect completely from the operating system and going serverless.

While container technology like LXC, Docker, and Kubernetes is completely free and open source - serverless environments on the other hand is not! [AWS Lambda][aws-lambda], [Google Cloud Functions][google-cloud-functions] and [Azure Functions][azure-functions] all tries to tie you as close to the cloud provider's proprietary runtime and services as possible.

One of the reasons container technology became mainstream was accessebility and portability - anyone could use it for free and as long as you had the container runtime installed it didn't care what infrastructure it ran on - Serverless has to be the same in order to achieve the same! 

The @knative project is that abstraction! Knative extends Kubernetes to provide a set indepently usable components like _build_, _serving_, and _eventing_ for modern, source-centric, and container-based apps that can run anywhere.

![The Knative Stack](/assets/image/2019-06-30-knative-stack.jpeg)

### Knative Serving

Run serverless containers on Kubernetes with ease, Knative takes care of the details of networking, autoscaling (even to zero), and revision tracking. You just have to focus on your core logic.

The Knative Serving project provides middleware primitives that enable:

- Rapid deployment of serverless containers
- Automatic scaling up and down to zero
- Routing and network programming for Istio components
- Point-in-time snapshots of deployed code and configurations

![Knative Serving](https://raw.githubusercontent.com/knative/serving/master/docs/spec/images/object_model.png)

<small>**Source:** https://github.com/knative/docs/tree/master/docs/serving</small>

**Service**: manages the whole lifecycle of your workload. It controls the creation of other objects to ensure that your app has a route, a configuration, and a new revision for each update of the service. Service can be defined to always route traffic to the latest revision or to a pinned revision.

**Route**: maps a network endpoint to one or more revisions. You can manage the traffic in several ways, including fractional traffic and named routes.
 
**Configuration**: maintains the desired state for your deployment. It provides a clean separation between code and configuration and follows the Twelve-Factor App methodology. Modifying a configuration creates a new revision.

**Revision**: is a point-in-time snapshot of the code and configuration for each modification made to the workload. Revisions are immutable objects and can be retained for as long as useful. Knative Serving Revisions can be automatically scaled up and down according to incoming traffic. See Configuring the Autoscaler for more information.

#### Hello World

```yaml
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: hello-world
  namespace: default
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: my/image
```

### Knative Eventing

Universal subscription, delivery, and management of events. Build modern apps by attaching compute to a data stream with declarative event connectivity and developer-friendly object model. 

### Knative Build

Provides easy-to-use, simple source-to-container builds, so you can focus on writing code and know how to build it. Knative solves for the common challenges of building containers and runs it on cluster.
