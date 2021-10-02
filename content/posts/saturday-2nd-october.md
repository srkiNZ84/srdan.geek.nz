+++ 
draft = false
date = 2021-10-02T22:28:48+13:00
title = "What is Bazel?"
description = "Taking a first look at bazel.build"
slug = ""
authors = ["Srđan Đukić"]
tags = ["bazel", "build", "devops"]
categories = []
externalLink = ""
series = []
+++
# What is Bazel?

Recently I read [an article](https://eng.uber.com/go-monorepo-bazel/) from the Uber engineering team about how they are using Bazel on their Go monorepo. This prompted me to take a look at Bazel and what it does differently to other build systems.

[Bazel](https://bazel.build/) is an open source build and test tool that originally came from Google. The main reasons why you might want to use it instead of the plethora of other build/test tools out there is that is optimised to do dependency management between libraries and services/binaries, regardless of language or framework. As such, it is arguably the perfect tool if you are implementing a polyglot microservice architecture that you version in a single monorepo. Think being able to "fan out, fan in" efficiently across many different services.

To get started with Bazel, go through the [Java tutorial](https://docs.bazel.build/versions/4.2.1/tutorial/java.html) which explains the basic concepts of the tool (I really liked the "query" functionality to print out the dependency graph in GraphViz format). Once you've gone through the tutorial, you can then brose the [build rules](https://docs.bazel.build/versions/4.2.1/rules.html) to see whether there is existing support for your particular language/framework and maybe check out the [awesome bazel](https://awesomebazel.com/) in case there is no official support for rules relating to your langauge.

Coming back to the original article, while a lot of the work that the Uber team have been doing sounds awesome, I do find some aspects of how they're using Bazel strange. Specifically the bit about having Bazel come up with a dependency graph, export it (with hashes) to their SubmitQueue in order to be able to build changes "out of order". Nevertheless, Bazel does seem like a very useful tool withthe dependency graph concepts doing a great job at implementing the core ideas from the CD book (GoCD also did this very well). I feel like the success of Bazel will correlate with whether monorepo version control patterns become popular (or the norm?) in the future. Without this, it feels like it will remain a niche tool with a limited user base.
