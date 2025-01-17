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

````
x = 5
y = x
x,y
(5, 5)
y += 1

x,y
(5, 6)
````

In Python, similar to C and C++, the %i placeholder can be replaced by an integer within a string when handling arguments. This also applies to other supported types: %s for strings and %f for floating-point numbers.

````
"The %i %s cost %f euros" % (iAmounf, product, 17.49)
````
Python can handle ASCII characters using a 7-bit representation while also supporting Unicode, which allows for a broader range of byte usage. Additionally, it supports the use of **QString** when working with the **PyQt** framework. String slicing properties in Python are defined as **[x:y]**, where x specifies the starting position of the substring from the left, and y specifies the endpoint from the left. If the number is negative it starts from the right. For example, the operator [-3:] can be used to create a subset starting from the third character from the end of the string.

````
phrase = "The red balloon"
phrase[4:7]
'red'
````

