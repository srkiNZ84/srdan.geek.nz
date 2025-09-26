+++ 
draft = true
date = 2025-09-26T22:03:11+12:00
title = "AI etc..."
description = ""
slug = ""
authors = ["Srđan Đukić"]
tags = []
categories = []
externalLink = ""
series = []
+++

Below is a chain of thoughts about LLMs and how I think we are missing the larger point.

# CRUD Monkeys

At some point most developers realise that there is a vast disparity between the algorithms and data structes taught in
Computer Science classes and the types of tasks that they carry out day to day. There are a very limited number of
programmers that get to implement interesting algorithms or work with scaled systems. The rest of our profession mostly
just re-implements known solutions with some small tweak or change. A lot of the time these are re-implemented rather
poorly, losing a lot of the knowledge from previous successful attempts to solve the problem.

# Swardley mapping and duplication

Nobody in our industry really likes to talk about this. While there is no shortage of people outraged and willing to go
to great lengths to decry the amount of duplication and waste in government, people seem to be a giant blind spot when
it comes to taking a look at that same problem in the private industry.

The one time that I did see it addressed directly was in the writings of [Simon
Wardley](https://en.wikipedia.org/wiki/Simon_Wardley).

# Open Source

One of the aims of the Open Source movement was to try and reduce, mitigate or at least provide a sane alternative to
this situation where the same functionality is re-implemented over and over again. (i.e. "why should I keep paying every
year for a word processor?").

The mechanism for this was to provide source code (and derived binaries, packages etc...) over the internet. Programmers
also tried their best to create the most useful "unit" of software e.g. library, packge, binary or product

# The discovery problem

Being a programmer faced with a problem and having realised the above, you could be fairly certain that:

* Someone had previously run into this same problem before
* Someone had solved this problem using code before
* Someone had made the source code for solving the problem available as open source on the internet

However, then the problem becomes how to "find" the above. This is the problem that search engines and forums like
StackOverflow solved for a long time.

# The workflow

So, with the above, the workflow becomes something like:

* Analyse the problem (what needs to be done?)
* Come up with a plan or two to tackle the problem (I could write a function to do this or I could look for an existing
Python SSO library)
* Figure out which of the plans we are going to follow (which is quicker? will lead to more maintainable systems?
etc...)
* Search the internet (using Google, StackOverflow, GitHub etc...) trying to determine if someone has made the source
available for the solution we are trying to build and if so, is it in the right "format" (language, unit of code etc...)
we need.
* Implement the solution as required


# Database of open source with a natural language interface

LLMs scanned "the internet", specifically:

* GitHub
* StackOverflow
* Computer Science textbooks
* Several other sources

and dumped them into a "model" (can think of it as a DB) with a natural language interface. This allows someone using
the LLM to completely bypass the discovery phase (the "search") and partially bypass the "planning" stage.

# Speed of solving already solved problems increases

So, we end up with a marginal increase in the amount of time it takes to implement solutions to problems already solved.
That is not particularly worthless in itself, however it falls short of the amount of value that the industry says it
will generate (in increased productivity? do we really need more digital products?).

Furthermore, what bothers me more than anything is that now the duplication and waste problem is laid bare for all to see
(i.e. see how little of our daily work is original) people seem to prefer to pretened that we have created "digital
gods" than look in the mirror.
