---
layout: post
title: Qt Container and Iterators
excerpt: Article about Qt Container and STL containers
modified: 5/07/2021, 9:00:24
tags:
  - Cpp, OOP, Qt, Iterators
comments: true
category: blog
---
# Container
**Containers** in the **STL** are C++ template classes designed to store objects or pointers. These containers are categorized into two main types: **sequential containers** and **associative containers**.

## Sequential Containers
Sequential containers can be classified as standard and non-standard, with standard containers being the most commonly used. Some notable examples include:

1. **Vector**: A sequential memory container that is typically defined with an initial size. While it can be resized, this process is costly due to the need to reallocate memory and copy elements.
2. **String**: Essentially a vector<char> with additional functionality specifically designed for handling character strings, such as concatenation, searching, and text manipulation.
3. **Deque**: A container similar to a vector but with the ability to efficiently expand at both ends, making it ideal for operations at the front and back of the sequence.
4. **List**: A doubly linked list that does not store elements contiguously. It allows quick modifications, such as insertions and deletions at any position, but operations like sorting and iteration are more expensive compared to a vector due to its non-sequential memory structure

## Associative Containers
Associative containers can be classified as standard and non-standard, with standard containers being the most commonly used. Some notable examples include:

1. **Map**: A collection where the key and value can be different types. Keys must be unique.
2. **Set**: A collection where the key and value are the same, effectively storing unique elements. Keys must also be unique.
3. **Multimap**: Similar to a map, but allows multiple entries with the same key.
4. **Multiset**: Similar to a set, but allows multiple elements with the same value (duplicate keys).

# Iterator

# Reference
[1] Ezust, A., & Ezust, P. (2006). An Introduction to Design Patterns in C++ with Qt 4. Prentice Hall. ISBN-10: 0131879057, ISBN-13: 978-0131879058.
