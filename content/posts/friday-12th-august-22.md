+++ 
draft = false
date = 2022-08-12T10:19:26+12:00
title = "Friday 12th August 22"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

* HackerRank [electronics shop problem](https://www.hackerrank.com/challenges/electronics-shop/problem):
```
func getMoneySpent(keyboards []int32, drives []int32, b int32) int32 {
     maxSpend := -1
     
     for _, k := range(keyboards){
         for _, d := range(drives){
             price := int(k + d)
             if price <= int(b) && price > maxSpend {
                 maxSpend = price
             }
         }
     }
    return int32(maxSpend)
}
```
Surprisingly got this on the first go. HackerRank using "int32" everywhere is annoying as it means typecasting all over
the place

* HackerRank [cats and a mouse problem](https://www.hackerrank.com/challenges/cats-and-a-mouse/problem):
```
func catAndMouse(x int32, y int32, z int32) string {
    aDist := math.Abs(float64(z - x))
    bDist := math.Abs(float64(z - y))
    
    if (aDist == bDist){
        return "Mouse C"
    } else if (aDist < bDist) {
        return "Cat A"
    } else {
        return "Cat B"
    }
}
```
This one was frustrating due to type casting as well, but not because of HackerRank, but because of Go's math.Abs
function, which apparently can't figure out what to do with an int? Though, it looks like it might have had auto-casting
before, but no longer does.

* Did my daily mindfulness meditation on Headspace app

* Did my Spanish lessons
