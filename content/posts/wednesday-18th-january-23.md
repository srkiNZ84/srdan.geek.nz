+++ 
draft = true
date = 2023-01-18T10:57:28+13:00
title = "Go concurrency exercises"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
# Go concurrency exercises

One of the random things that I do when I'm procrastinating is open up github.com and browse the "For you" feed (not
sure if this is on for everyone), which is some kind of "feed" of updates and repositories that GitHub thinks you might
be interested in.

Occasionally there's something interesting in there, worth taking a look at either right away or later. Today was one of
those days where it recommended the repository https://github.com/loong/go-concurrency-exercises

This was a pretty nice, real world set of exercises based off of the standard Go concurrency libraries.

I solved the first one before putting it aside and getting back to work.

go_concurrency_exercises_0.png

The first attempt I made the mistake of declaring the Timer channel within the Crawl function, which worked until the
"recursive" call was triggered. 
