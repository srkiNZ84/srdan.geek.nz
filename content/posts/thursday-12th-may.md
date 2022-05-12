+++ 
draft = true
date = 2022-05-12T14:12:44+12:00
title = "Snapd and why it is confusing"
description = ""
slug = ""
authors = ["Srđan Đukić"]
tags = [snapd]
categories = []
externalLink = ""
series = []
+++
# Snapd

So, I installed Docker on Ubuntu through the Snap. Running it causes issues because the running containers don't have
outbound connectivity. In trying to fix this one of the things I tried was to add DNS configuration to the Docker
daemon.

With snap this was a mess around. I eventually found that you had to modify the file under
/var/snap/docker/current/config. I wasted time trying to find this out and eventually got to the Snapcraft package
homepage: https://snapcraft.io/docker which described it.

Oh, also, the snap version works around SystemD, meaning you have to start/stop Docker with "sudo snap start docker"
etc...
