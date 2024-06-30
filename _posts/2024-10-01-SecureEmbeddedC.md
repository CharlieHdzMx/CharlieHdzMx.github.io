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
2. They can be directed to execute original code that triggers a preliminary failure, leading to system misbehavior.lkl
