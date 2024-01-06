---
layout: post
title: Liskov Substitute Principle
excerpt: LSP design
modified: 5/07/2021, 9:00:24
tags:
  - Design Patterns
  - Architecture
comments: true
category: blog
---
LSP, or the Liskov Substitution Principle, is an important feature for all software entities that conform to the Open-Closed Principle, which was introduced by Barbara Liskov. The principle is stated as follows:

_"What is wanted here is something like the following substitution property: If for each object **o1** of type **S** there is an object **o2** of type **T** such that for all programs **P** defined in terms of **T**, the behavior of **P** is unchanged when **o1** is substituted for **o2**, then **S** is a subtype of **T**."_
