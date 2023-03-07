+++ 
draft = true
date = 2022-05-24T10:30:35+12:00
title = "Parsing JSON in Python vs Perl"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
# Parsing JSON in Python vs Perl

So, one of the issues that comes up fairly regularly in SRE/DevOps circles is "which language" to use for a particular
problem. The choices are generally one of:

* Bash
* Python
* Perl
* Go

(NOTE: This is not an exhaustive list, but just from anecdotal evidence, these seem to come up most often)

One argument that came up recently on the job (where there was existing Perl code) was made that parsing JSON was
easier in Perl than Python, which was quite strange to me. Even though JSON originated with Javascript, Python has had
pretty much first class JSON support for as long as I can remember. Perl has good JSON support, but it's clear that the
JSON protocol came along well after the language was created and is treated in the same vein (rightly or wrongly) as
"just another protocol" (i.e. no different from XML).

So, for my own mental health, I'm going to write some code comparing JSON parsing in Perl and Python, focussing mostly
on conciseness and ease of writing/reading the code (I'm not particularly interested in performance).

## The problem



## Perl solution

## Python solution

## Conclusion


https://www.xmodulo.com/how-to-parse-json-string-in-perl.html

https://www.programiz.com/python-programming/json
