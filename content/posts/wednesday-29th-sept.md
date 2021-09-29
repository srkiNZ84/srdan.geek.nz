+++ 
draft = false
date = 2021-09-29T21:59:32+13:00
title = "Docker Container image scanning"
description = "Discussion of the state of container image scanning at the current time"
slug = ""
authors = ["Srđan Đukić"]
tags = ["docker", "scanning", "security", "acr", "azure", "azure defender", "container registry", "patching", "distroless", "clair", "trivy", "grype"]
categories = []
externalLink = ""
series = []
+++
# Docker Container image scanning

Some time ago, I wrote [an article](https://clearpoint.digital/devops/patches-arent-just-for-pirates-%E2%98%A0-vulnerability-scanning-docker-images-with-coreos-clair-and-klar/) talking about the state of container scanning as it became clear that container images were going to have to be patched and the process was going to have to be managed just the same as we do for Linux boxes. 

Recently, the question came up again at work after a colleague rightly balked at the integrated Azure Container Registry option (Azure Defender for ACR) [costing ~$0.44NZD every time you want to scan an image](https://azure.microsoft.com/en-gb/pricing/details/azure-defender/). As such, this post is an attempt to summarise the state of container scanning and the avaible options at time of writing, with a focus on open source solutions that are easy to use and integrate into CI.

(Clair)[https://github.com/quay/clair] is still a good option these days, though it feels like it has been somewhat surpassed by other options in terms of ease of use and integration. Since the last time writing about it, its parent company, CoreOS, has been purchased by RedHat, who seem to be doing a good job of maintaining the project as a part of the (quay.io)[https://quay.io] open source container image registry effort.

A slightly easier to use option that seems to have fit the brief in the end is a tool called [Trivy](https://github.com/aquasecurity/trivy) from (Aquasecurity)[https://www.aquasec.com/]. This seems to do the job of detecting known vulnerabilities in package versions as well as offering some additional scanning for common misconfigurations and was easy to integrate with Azure DevOps.

A third option that came up but that we didn't really get a chance to take for a spin was [Grype](https://github.com/anchore/grype) from [Anchore](https://anchore.com/). This also looks promising in terms of easy of use (as well as install, configuration and integration with CI) with an added benefit that it claims to also scan language packages (Ruby, Java, JavaScript and Python at the moment).

In addition to the above, it was also noted that both [Quay.io](https://quay.io/) and [Harbor](https://goharbor.io/) have security scanning built in (and possibly have a free tier?) and that DockerHub has this in [their paid subscription](https://docs.docker.com/docker-hub/vulnerability-scanning/) powered by [Snyk](https://snyk.io/).

In terms of eliminating the need to patch in the first place, it was recommended to look at migrating to the [Distroless](https://github.com/GoogleContainerTools/distroless) base images if possible in the future.
