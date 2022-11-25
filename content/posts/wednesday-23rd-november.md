+++ 
draft = false
date = 2022-11-23T15:21:18+13:00
title = "Zenva Object Oriented C++ course"
description = "Discussion of an online C++ course and the language itself"
slug = ""
authors = ["Srđan Đukić"]
tags = ["c++", "cpp", "zenva", "learning"]
categories = []
externalLink = ""
series = []
+++
# Zenva Learn Object-Oriented C++ by Building a Game

A while ago I bought a bunch of Zenva programming courses from [Humble Bundle](https://www.humblebundle.com/) as some of
the courses covered C++, which I hadn't touched since university, but was becoming a language that I was dealing with
semi frequently in my new role.

So, this was a "refresher" on the C++ world and where it had come to in the last 10 years or. The course I did can be
found here: <https://academy.zenva.com/product/learn-object-oriented-cpp-by-building-a-game/>

## Course impression

The course itself was fairly well organised in terms of being logical and easy to follow. I particularly liked the use
of [Replit](https://replit.com/) which was great and was my first introduction to the in browser IDE.

It did feel like for the author of the course, it might not have been his primary/first language.

## C++ impression

My impression of the C++ language after having not touched it for many years were not great. It feels like the language 
offers a massive amount of flexibility in terms of how to write particular bits of code/implement solutions, which
sounds good, but it comes at the expense of there not being a clear/idiomatic way to do things. An example of this is
that there are so many ways to include code files that the advice was to add "#ifndef LIB_FILE ..." at the top of each
file to prevent including the same libraries/files multiple times. 

Additionally, I found the syntax quite clunky (e.g. "->function()" vs ".function()") and overly verbose at times. 

There was also a bug where if a player picked up an "item", the definition of which looks like:
```C++
struct item {
  std::string name;
  int damage;
  int health;
};
```
and has an instance like (note that damage property is left uninitialised):
```C++
  item healingPotion;
  healingPotion.name = "Healing Potion";
  healingPotion.health = 50;
```
when you come to apply (or "pick up") the item:
```C++
void Player::pickUpItem(item item) { 
  damage += item.damage;
  heal(item.health);
}
```
for some reason the "damage" is increased by a random value (sometimes "6")?

I've created a public Replit reproducing the issue here: <https://replit.com/@srkiNZ/TestingCPPInt?v=1>

I didn't get to the bottom of this one, however, it's unexpected behaviour like this that doesn't give much confidence
around the safety of C++ as a language. The tradeoff is the performance and the portability.
