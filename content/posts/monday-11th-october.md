+++ 
draft = false
date = 2021-10-11T22:51:06+13:00
title = "Terraform and deduplication"
description = "Issues you can run into when defining things twice"
slug = ""
authors = ["Srđan Đukić"]
tags = ["terraform"]
categories = []
externalLink = ""
series = []
+++
# Terraform and deduplication

One of the issues I hit today was Terraform not picking up that I had declared an IAM policy for the same SNS topic in two different places. In fact it took me a long time to figure out what was going on as Terraform decided to use one of the policies and I couldn't understand why the changes I was making were not being reflected at plan/apply time.

After some time I finally figured it out and expressed my confusion on the company IM as this was something I was quite sure Terraform should have done for me. At this point I was really grateful to have really good colleagues to work with as they jumped in, trying to figure out the issue and eventually got to [this known Terraform issue](https://github.com/hashicorp/terraform-provider-aws/issues/14394) with the AWS Provider which lays out the problem quite nicely and gives some hope that if it becomes an issue for lots of people in the future it will get fixed.


