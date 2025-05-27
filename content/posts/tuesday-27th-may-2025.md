+++ 
draft = false
date = 2025-05-27T12:05:08+12:00
title = "Good article about graceful shutdown with Go"
description = ""
slug = ""
authors = ["Srđan Đukić"]
tags = []
categories = []
externalLink = ""
series = []
+++
# Good graceful shutdown article

Just finished reading a good article about [graceful
shutdown](https://victoriametrics.com/blog/go-graceful-shutdown/index.html) and OS
signal handling in [Golang](https://go.dev/).

Was really good and a reminder of all of the things we lerarned when first started
using Kubernetes in 2015-2016.

Good discussion about it on [HackerNews](https://news.ycombinator.com/item?id=43889610)
as well.

Made me think I want to develop a test suite for the graceful shutdown for the sample
code. Also lead to some other reading:

* New (to me) observability platform [Openobserve](https://openobserve.ai/)

* An interesting article on using [SIGHUP for configuration 
reloads](https://blog.devtrovert.com/p/sighup-signal-for-configuration-reloads)
