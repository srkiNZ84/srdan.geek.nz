+++ 
draft = false
date = 2021-09-28T22:01:55+13:00
title = "The rise of K8s for infrastructure management"
description = "More and more people are looking to use the Kubernetes API for management of cloud resources"
slug = ""
authors = ["Srđan Đukić"]
tags = ["kubernetes", "k8s", "iac", "crossplane", "operators", "cloud", "aws", "azure", "infrastructure"]
categories = []
externalLink = ""
series = []
+++
# The rise of K8s for infrastructure management

One of the typically overlooked aspects of moving to microservice architecture in general (and specificallyK8s for the purposes of this article) is the fact that each service "owns" its own datastore. Unless constrained by costs, this means a fully managed datastore instance (e.g. RDS). Once we accept that the service owns and manages the creation, upgrades etc... of its own datastore, it then becomes logical to store the "infrastructure code" for this next to the application code (i.e. in the same repository). This is good as it really gives the team responsible a sense of autonomy of their own infrastructure and, currently, likely looks like a bunch of Terraform files in a directory in version control, next to the application code.

The drawbacks of the above approach is that the team now has to learn Terraform, which can be a learning curve and has several concepts which don't fit nicely with the developer mindset (e.g. the heavy use of a "statefile"). If only there was a way to manage infrastructure the same way that we manage our deploy objects... 🤔

This is where tools like the [Azure Service Operator](https://github.com/Azure/azure-service-operator), [Crossplane.io](https://crossplane.io/) and [AWS Controllers for Kubernetes (ACK)](https://github.com/aws-controllers-k8s/community) come in. They allow a development team (or anyone really) to define their cloud infrastructure objects using Kubernetes API objects. 

For example, to define an Azure Cache for Redis instance using the Azure Service Operator, you would include the following in your Helm charts (or whatever tool you use to deploy):
{{< highlight yaml >}}
apiVersion: azure.microsoft.com/v1alpha1
kind: RedisCache
metadata:
  name: azure-redis
spec:
  location: eastus2
  properties:
    sku:
      name: Basic
      family: C
      capacity: 1
    enableNonSslPort: true
{{< /highlight >}}

To define an AWS CloudFront distribution using Crossplane, you would do the following:
{{< highlight yaml >}}
apiVersion: cloudfront.aws.crossplane.io/v1alpha1
kind: Distribution
metadata:
  name: example-distribution
spec:
  forProvider:
    region: us-east-1
    distributionConfig:
      enabled: true
      comment: Example CloudFront Distribution
      origins:
        items:
          - domainName: test-bucket.s3.amazonaws.com
            id: s3Origin
            s3OriginConfig:
              originAccessIDentity: ""
      defaultCacheBehavior:
        targetOriginID: s3Origin
        viewerProtocolPolicy: allow-all
        minTTL: 0
        forwardedValues:
          cookies:
            forward: none
          queryString: false
  providerConfigRef:
    name: example
{{< /highlight >}}

The above is excellent! It makes brilliant use of the K8s control loop to ensure we have our declarative infrastructure, while also re-purposing the K8s API, extending it in a way that's natural and useful. It also does this without forcing developers to learn a new DSL (the presumption being that everyone knows YAML).

Out of the three listed, Crossplane looks the most polished at the moment, however, it is targetting several clouds and has features like [composition](https://crossplane.io/docs/v1.4/reference/composition.html), so might feel "heavy" when compared to the "native" offerings. In saying that, it seems to have much more support, in terms of infrastructure objects, on AWS than ACK. Another alternative to the above tools might be [OperatorHub.io](https://operatorhub.io/), which seems to be a website that aggregates K8s operators and makes them easy to find. This option might makes sense when there is a high trust environment and teams are able to install their own Operators.

These tools give a way to use the same workflow and tools for handling your applications' managed cloud infrastructure that you already use for your "deployment" infrastructure (i.e. Deployments, Services, ConfigMaps and Secrets). As such, they continue to push for greater team autonomy, while also having safety benefits by allowing infrastructure and application changes to be versioned, tested and deployed together.
