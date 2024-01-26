+++ 
draft = true
date = 2024-01-19T16:36:09+13:00
title = ""
description = "Dynamic piplines in GitLab CI"
slug = ""
authors = ["Srđan Đukić"]
tags = ["gitlab", "ci", "gitlab-ci", "pipelines", "monorepo"]
categories = []
externalLink = ""
series = []
+++
# Creating and using dynamic pipelines in GitLab CI

In some cases you want to dynamically generate build jobs depending on your repository structure. If this is the case,
one option is to use GitLab CI to generate and add the build jobs at "run time".

This post shows how to do that using Python

<https://gitlab.com/srkiNZ/dynamic-pipeline-demo>

(also documented here: <https://docs.gitlab.com/ee/ci/pipelines/downstream_pipelines.html#dynamic-child-pipelines>)
