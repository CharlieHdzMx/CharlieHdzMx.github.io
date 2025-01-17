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
# Collections
In Python, collections are containers of references. Unlike C++, these containers can be **heterogeneous**, meaning they can hold references to **different data types**. The most important collections in Python are:

1. **Tuple**: Immutable sequential containers.
2. **List**, Mutable sequential containers.
3. **Dict** (Dictionary),
4. **Set**,
5. **Frozenset**,

## Tuples
To create a tuple, you can use _tuple = ()_. If you want to initialize a tuple with elements, you can include them separated by commas inside the parentheses, such as _tuple = (1, 2, 3)_. Tuples can be sliced in the same way as strings. Additionally, individual elements can be accessed using index operators.

## List
Similar to tuples, lists are sequential containers, but unlike tuples, they are **mutable**. This means that lists can be modified by adding, removing, or changing their elements. Lists are created using square brackets **[] ** instead of parentheses (), which is the main difference in their declaration compared to tuples. Lists provide several useful methods for manipulation:

The **insert(x, y) **function allows you to insert a value y at a specific index x.
To remove an element by its index, you can use the **del listName[x]** statement, where x is the index of the element to be removed.
The remove("value") method can also be used to delete an element by specifying its value.

Since lists (and dictionaries) are mutable, any changes made to a variable that references the same object will affect all related variables. For example, if x = y and you modify x, the changes will also reflect in y. This behavior occurs because both variables point to the same underlying object in memory.
