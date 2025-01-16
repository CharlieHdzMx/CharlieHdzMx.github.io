---
layout: post
title: Introduction Python
excerpt: An introduction Python
modified: 1/02/2019, 9:00:24
tags: [Python]
comments: true
category: blog
---

# Introduction

Python provides two ways to compare objects: by identity and by value. Value is compared using operators such as == and <, while identity refers to the memory addresses of the objects, which can be accessed using the id() function. Additionally, objects can be compared with the unique object None to check for their existence. Python does not allow comparison between objects of different types, raising an exception in such cases. Variable names must begin with a letter and are case-sensitive. By default, numbers and strings are immutable. This means that when an assignment is made based on another object, the referenced object does not change. As a result, changes by reference are not allowed by default.

``
x = 5
y = x
x,y
(5, 5)
y += 1

x,y
(5, 6)
``
