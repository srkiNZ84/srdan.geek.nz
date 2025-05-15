+++ 
draft = false
date = 2025-05-15T16:21:23+12:00
title = "Efficiency of Lambda languages"
description = "Interesting article about how much of a difference language efficiency makes for Lambda costs"
slug = ""
authors = ["Srđan Đukić"]
tags = ["aws", "lambda", "languages", "efficiency", "costs"]
categories = []
externalLink = ""
series = []
+++

# Efficiency of Lambda languages

Today I read [this article](https://xebia.com/blog/aws-lambda-benchmarking/) in which the author designs a (fairly)
simple system of Lambda functions and then proceeds to implement the system functions using different languages. I
believe that the aim of the "experiment" is to:

* verify whether the choice of language makes a difference to the costs of the system
* if so, to what extent it makes a difference

The languages compared are Scala, Python, Rust and TypeScript, mainly looking at the associated Lambda costs for
each system/language.

The results are fairly predicatable with respect to the first point, language choice does make a difference in
terms of running costs. The surprise for me at least was by "how much". The difference between Rust and the other
languages was massive.

Another surprise for me was how much more cost efficient Python was compared to TypeScript (I had assumed that
the performance of the two languages would be similar).
