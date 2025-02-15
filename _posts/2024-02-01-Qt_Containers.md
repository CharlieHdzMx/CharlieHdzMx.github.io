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
# Composite Pattern 
Composition allows clients to treat **individual objects and compositions of objects uniformly**.

The **Composite Pattern** demonstrates how to use recursive composition to prevent clients from having to handle objects differently based on their primitive types or requiring different ways to contain those objects.

The key to the Composite Pattern is having an **abstract class that represents all primitive objects and their containers**. This abstract class also declares the operations that all composite objects will use. For example, in a GUI with different shapes, a function like _draw()_ would be implemented uniformly for all shapes, such as _Circle_, _Rectangle_, or _Triangle_.

In this case, a class called _Figure_ would be defined as an abstraction for all shapes. This Figure class would then be used as the element of any container in the system, but specifically as a pointer (or reference). Using pointers ensures that the abstraction retains information about the specific type of Figure at runtime. If regular objects were used instead of pointers, there could be a loss of data or information about the original shapes.

https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/blob/9a48496e07f5e07487df1c6f39afa7714b6fb808/images/CompositePattern/CompositePattern1.png

## QObject
**QObject** is the base class for most Qt classes and implements the **Composite Pattern**, while also using signals and slots as part of the _Observer Pattern_. The Composite Pattern involves creating complex components through a tree-like hierarchy structure of simpler components, ensuring that clients cannot distinguish between complex and simple components.

There is a difference of what is a compose of different objects and what are the elements of a composition. The definitons are:
**Composite Object:** An object that contains child objects.
**Component Object:** An object that has a parent object.

QObject serves as both a composite and a component because it can reference another QObject as its parent. The highest level of composition in QObjects represents the root, while derived objects act as leaves. A QObject has a method called _setParent()_, which allows associating it with another QObject as its parent. Additionally, it provides recursive functions like _findChildren()_, which returns a QList of pointers to all its child objects. QObjects without parents are placed on the stack, whereas QObjects with parents are placed on the heap.

# Container
**Containers** in the **STL** are C++ template classes designed to store objects or pointers. These containers are categorized into two main types: **sequential containers** and **associative containers**.

A relationship between containers and objects can be defined as a **managed container** when objects are used as the elements of the container. In this case, when **the container is destroyed, all its elements are also destroyed.** This type of relationship is known as composition. Alternatively, an **associated container** can be identified when, instead of storing objects directly, the container holds pointers to objects. In this scenario, when **the container is destroyed, the objects themselves are not destroyed** because the container only stores references (pointers) to those objects. This type of relationship is referred to as association.

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

# Qt Containers
Like some of C++'s non-standard containers, Qt also defines several containers that can be used, such as:

1. **QStringList**: A class derived from QList<QString>, specifically designed for handling lists of strings.
2. **QHash**: An associative container that uses a hash table to enable fast key lookups.
3. **QCache**: An associative container optimized for quick access to recently used data while automatically removing items that are no longer frequently accessed.

# Iterator

# Reference
[1] Ezust, A., & Ezust, P. (2006). An Introduction to Design Patterns in C++ with Qt 4. Prentice Hall. ISBN-10: 0131879057, ISBN-13: 978-0131879058.
