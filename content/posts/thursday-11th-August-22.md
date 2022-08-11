+++ 
draft = false
date = 2022-08-11T11:21:35+12:00
title = "Thursday and daily blogging"
description = "Thursday 11th of August post"
slug = ""
authors = ["Srđan Đukić"]
tags = ["daily", "duolingo", "hackerrank"]
categories = []
externalLink = ""
series = []
+++
# A random Thursday in August

So, in order to try and slay my own demons of procrastination and a general feeling of not achieving anything, I've
decided to start blogging regularly about all the big and small achievements every day.

Today I:
* Solved two basic HackerRank exercises:
- [Drawing Book](https://www.hackerrank.com/challenges/drawing-book/problem)
```
func pageCount(n int32, p int32) int32 {

    numBuckets := n/2
    
    bucketNumber := p/2
    
    wayFromBack := numBuckets - bucketNumber
    if (bucketNumber < wayFromBack){
        return bucketNumber
    } else {
        return wayFromBack
    }
}
```
Which was surprisingly easy in the end, but for some reason I massively over-thought it when I first looked at it. Also
"buckets" was a weird choice of naming looking at it now.

- [Counting Valleys](https://www.hackerrank.com/challenges/counting-valleys/problem)
```
func countingValleys(steps int32, path string) int32 {
    altitude := 0
    
    pathSplit := strings.Split(path, "")
    inValley := false
    numValleys := 0
    for _, ch := range(pathSplit) {
        if ch == "U" {
            altitude++
            if altitude >= 0 {
                inValley = false
            }
        } else if ch == "D"{
            altitude--
            
            //Check if we are already in a valley and if we are below sea level
            if !inValley && altitude < 0 {
                numValleys++
                inValley = true
            }
        } else {
            fmt.Println("Unknown character " + ch)
        }
    }
    return int32(numValleys)
}
```
This one I found more straight forward, though on first go at it, I forgot to change the status of "inValley" at the
appropriate points.

* Did two Duolingo exercises (I'm learning Spanish)

* Did a workout and some [football handling exercises](https://youtu.be/U3N_qXaqrtI)

* Found out how to connect to VNC on the latest OS X server (in [Remmina](https://remmina.org/) you have to turn off
  encryption). [XTightVNC](https://www.tightvnc.com/) and [TigerVNC](https://tigervnc.org/) work fine, but are slower
