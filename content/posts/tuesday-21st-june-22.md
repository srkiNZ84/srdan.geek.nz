+++ 
draft = false
date = 2022-06-21T09:21:11+12:00
title = "Read the requirements carefully"
description = "Verbose desciption of the divisible sum pairs problem"
slug = ""
authors = ["Srđan Đukić"]
tags = ["hackerrank", "problem solving"]
categories = []
externalLink = ""
series = []
+++
# Reading the requirements

So, last night I couldn't sleep and decided to hop onto hackerrank.com and have a go at attempting the [divisible sum
pairs](https://www.hackerrank.com/challenges/divisible-sum-pairs/) problem:

> Given an array of integers and a positive integer *k*, determine the number of (*i*, *j*) pairs where *i* < *j* and *ar[i]* + *ar[j]* is divisible by *k*.

It looked fairly straight forward and in fact I was pretty sure that I had attempted a problem like it previously. So, I
had a go at a first draft of the code, had some simple loop issues, to be expected and then passed the first test case!
Yay!

At this point I submitted my solution and... failed. Failed in a weird way where it was random test cases. At this point
I had a look at my logic, checking:
* Off by one issues
* Division issues
* Output statements

and still nothing! Getting frustrated, I then tried rewriting it with a "naive" algorithm, but the problem still
persisted.

At this point I used some of my HackerRank "points" to be able to download the input and expected answer from the
failing test cases and due to the limitations on the HR web UI, ran it on my laptop in VS Code. It looked like a "one
off" error of some sort as the answer was "216" and my code kept on producing "215".

More debugging and modifying the code further, including to sort the input array to try and make it easier to see where
the issue was and still no luck.

At this point, it was getting close to "wake up and go time" so I gave up and googled the solutions to the problem. The
results were generally good and all seemed straight forward and then it clicked!

> where *i* < *j*

this part of the problem description! I had interpreted it wrongly as:

> where *ar[i]* < *ar[j]* 

i.e. comparing the *values* of the array indicies rather than the actual *place* in the array

and had added an uncessary check to check whether the values were identical and if so, to not increment the pair
counter.

I removed the check and it worked.

## Lessons learned

I guess the lessons learned are to:
* Read the requirements or problem description carefully
* If you wake up in the night, have a cup of tea and go back to sleep
