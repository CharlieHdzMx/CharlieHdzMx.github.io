---
layout: post
title: C++ Intro and C++ Templates intro
excerpt: Article about C++ introduction adn thre concepts of templates on C++
modified: 5/07/2021, 9:00:24
tags:
  - Cpp
  - Templates
comments: true
category: blog
---
C++ is a versatile, multi-paradigm programming language commonly referred to as a compiled language. In a typical C++ program, various source files are compiled by a compiler, producing object files. These object files are then combined by a linker to generate an executable program. The following figure illustrates the process of creating an executable file in C++:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/964ddcd8-08f2-42b9-b803-42463cc75385)

In C++, there are two main types of entities:

1. **Core Language Features:** This includes built-in types, loops, conditional statements, and basic keywords that are fundamental to the language itself.
2. **Standard Library Components:** These are components from the Standard Template Library (STL), encompassing containers, algorithms, I/O operations, and essentially everything that is added using the `#include` directive to incorporate functionality from the standard library.

# Practical Introduction
For example, the following code snippet illustrates how to declare a function, utilize the standard output stream (cout), and provide a basic implementation of the main function, which serves as the entry point for the executable program.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/d0b122f4-9274-40ea-a35c-c49a99d926a2)

# Type, Variables & Arithmetic
Some fundamental C++ data types include:

- **bool:** Represents true or false values.
- **char:** Represents individual characters, such as 'a', 'f', or '&'.
- **int:** Represents integer values, like 1, 5, or 1245.
- **double:** Represents floating-point numbers, for example, 3.1415.

It's important to note that the size of these types can vary across different machines, and their sizes can be determined using the sizeof operator.

For instance, a typical size for a char is 8 bits, and the sizes of other variables are usually quoted in multiples of the size of a char. If a char is 8 bits, an int is commonly 32 bits, and a double is often 64 bits. The following image illustrates this concept:

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/d01644af-754f-4791-a612-c64a65c4e20d)

## Universal Initializer
With the advent of C++11, the syntax options for object initialization have expanded, but **braced initialization** is the most recommended and versatile syntax. This type of initialization helps prevent narrowing conversions and unintended calls. The following code snippet illustrates this idea:

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/ee683205-c9aa-4caa-ae9a-f4c10b987e70)



