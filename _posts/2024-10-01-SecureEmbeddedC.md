---
layout: post
title: Secured Embedded C
excerpt: Article about Secured Embedded C
modified: 5/07/2024, 9:00:24
tags:
  - Cybersecurity
  - C
comments: true
category: blog
---

# Pointer Subterfuge
Function pointers can be overwritten to transfer control to an attacker's code, causing issues in the software. When the program executes the function pointer call, the attacker's code runs instead of the original code.

Function pointers can also be modified when they are the target of an assignment, allowing attackers to control the address and alter other memory locations.

Function pointers can be vulnerable in two main ways:
1. They can be directed to execute code from an attacker, compromising system integrity.
2. They can be directed to execute original code that triggers a preliminary failure, leading to system misbehavior.

# Buffer overflow exploit.
There are several exploits that can be used to overwrite function pointers, with **buffer overflows** being one of the most common. These buffer overflows frequently occur due to a failure to restrict writing within array boundaries, often caused by improperly bounded loops.

For a buffer overflow to affect a function pointer, the buffer must be located in the same segment as the target function pointer. Executables typically have two types of data segments: **Data** and **BSS**. The Data segment contains all initialized global variables and constants, while the BSS segment contains all uninitialized global variables.

