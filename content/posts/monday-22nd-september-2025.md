+++ 
draft = false
date = 2025-09-22T22:23:26+12:00
title = "JavaScript security"
description = ""
slug = ""
authors = ["Srđan Đukić"]
tags = []
categories = []
externalLink = ""
series = []
+++

# Shai-Hulud

So, recently there's been a fairly nasty worm in the NPM ecosystem called
[Shai-Hulud](https://socket.dev/blog/ongoing-supply-chain-attack-targets-crowdstrike-npm-packages). (NOTE: the name
comes from [Dune](https://dune.fandom.com/wiki/Shai-Hulud) where it's the original name for the giant sand worms).

The Shai-Hulud, and other attacks, have prompted the increased popularity of documents such as [this gem of an best
practice document](https://github.com/bodadotsh/npm-security-best-practices) to become popular.

For me, this has become an incredibly sad reality. The NPM package repository, in its quest to claim the title of
"biggest" (in terms of the number of packages) seems to have thrown away so much of the utility of open source package
repositories. This utility was basically "trust". I.e. people used it because there was some sort of guarantee that the
code published could be trusted.

Whether it is NPM actively trying to encourage people to publish small packages or the JavaScript community having a
predisposition to smaller and smaller units of published code, the result seems to be a lot of code of dubious utility
and quality with lax policies. This isn't a particularly new problem and has been building up for a long time. It is
somewhat of a surprise for me as I just hadn't realised how bad things were.

# Impacts

So what are the upstream impacts? I mean, none of the problems with NPM are particularly novel. Really, the only novelty
is in the increased rate at which previously uncommon security issues are being exploited.

This is where I have a somewhat different opinion to others, in that it seems to me that the field of information
security has moved from a "theoretical" mindset (i.e. how can we make sure that we can't be broken into) to a pure
"risk management" mindset (i.e. how can we make it difficult enough to break in that attackers won't bother).

Looking at it from a "risk management" mindset, it's not particularly interesting whether or not a dependency injection
attack can happen or if it has happened before, but rather the rate at which it happens matters a lot.

What does this mean in practice? So, for a long time I have been advocating for "continously upgrading" your development
dependencies as a way to both:

* Create more secure software (through quickly and regularly pulling in security fixes)
* Keeping the technical debt to a minimum

I am not so sure about the first point anymore. It looks almost as if dependency injection attacks are becoming so
common now (at least in the JavaScript/npm ecosystem) that it is safer to "pin and never/rarely upgrade". 

To me, that is incredibly sad. That the open source ecosystem that spawned essentially all of JS, NodeJS, NPM etc... is
now deemed to be "somewhat untrusted". 

When I see package managers adding security features like "delay upgrades", it feels like a failure. If only for the
theoretical questions it poses like "why did you trust the version you pulled in the first place?". 

Anyway, it is what it is. I'll just have to [swallow a dead
rat](https://www.stuff.co.nz/business/105611924/budget-buster-the-art-of-swallowing-dead-rats) and accept that we will
all be maitaining a whole pile of JavaScript in 5-10 years time with massively out of date dependencies. 

NOTE: There is also a good discussion of this on [hacker news](https://news.ycombinator.com/item?id=45326754)
