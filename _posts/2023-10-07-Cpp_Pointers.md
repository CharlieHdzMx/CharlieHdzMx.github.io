---
layout: post
title: C++ Pointers
excerpt: Article about C++ Pointer main features
modified: 5/07/2021, 9:00:24
tags:
  - Cpp
comments: true
category: blog
---

**Pointers**, akin to Java references, are fundamental language mechanisms used to reference memory. Primarily, they enable the efficient passing of potentially large data sets. However, the improper implementation of pointers poses a significant risk of memory corruption. As a precaution, certain implementations may benefit from using **STL** containers instead of pointers. A notable advantage of STL containers is their built-in mechanisms for enhanced control over object addressing, thereby mitigating the risk of memory corruption.

Foremost among its benefits, using pointers allows us to reference the address of a specific object rather than passing the entire object, which can be notably resource-intensive.

The primary function of a pointer revolves around indirection, facilitated by the dereferencing unary prefix operator **(*)** that enables us to reference the object pointed to by the pointer. In addition to indirection, the address-of operator **(&)** serves to directly obtain the address of a particular object. To exemplify, the following snippet illustrates the usage of both the indirection and address-of operators:

