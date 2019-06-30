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

![/assets/image/2019-06-30-knative-stack.jpeg](The Knative Stack)

* knative build
* knative serving
* knative eventing
* links to workshop