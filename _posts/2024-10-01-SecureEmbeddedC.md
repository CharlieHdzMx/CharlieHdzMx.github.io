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
# Secure code
Secure software does precisely what it is supposed to do. This means it starts in a secure state and cannot transition to an insecure state. Achieving secure code involves learning from past security issues and applying state-of-the-art techniques tailored to specific systems or programming languages, all while maintaining the highest possible code quality.

As C is one of the most popular programming languages, many attackers exploit its vulnerabilities. The reason there are so many vulnerabilities in C and C++ is that these languages trust the programmer to know what they are doing. They aim to be small and simple, prioritizing speed but not guaranteeing portability.

C defines different behaviors for portability issues that can create opportunities for security attacks. Most security vulnerabilities result from Undefined Behavior and Unspecified Behavior.

1. **Undefined Behavior (UB):** Behavior caused by ambiguous failures.
2. **Unspecified Behavior (USB):** Behavior caused by ambiguous failures.
3. **Implementation-Defined Behavior (IDB):** Behavior dependent on the compiler implementation.
4. **Locale-Specific Behavior (LSB):** Ambiguities based on dependencies with locale-specific functions.

# CIA Triad
## Confidentiality.
**Confidentiality** is ensured when a system guarantees that unauthorized acquisition of information is not possible. The principal methods to protect confidentiality are based on data encryption.

## Integrity
**Integrity** is ensured when a system guarantees that unauthorized subjects cannot manipulate data without being detected. To prevent data manipulation and preserve data integrity within the system, Message Authentication Codes (**MACs**) and **Digital Signatures** are used. The sub-goals of integrity include:

1. **Correctness:** Data has not been modified. Data may appear complete, but some bytes might have been altered.
2. **Completeness:** Data has been fully transferred. Data may be correct, but if not entirely sent, it can cause system misbehavior.
3. **Freshness:** Data is not outdated. Data may be complete and unmodified, but if too much time has passed, it may be risky to use as it could be obsolete.

## Availability
**Availability** is ensured when a system enforces that authenticated and authorized subjects are not denied access by an unauthorized attacker. This can occur when system availability is reduced due to high resource consumption during peak times or through malicious overload attacks.

# Memory segments


# Stack buffer overflow
-
# Pointer Subterfuge
Function pointers can be overwritten to transfer control to an attacker's code, causing issues in the software. When the program executes the function pointer call, the attacker's code runs instead of the original code.

Function pointers can also be modified when they are the target of an assignment, allowing attackers to control the address and alter other memory locations.

Function pointers can be vulnerable in two main ways:
1. They can be directed to execute code from an attacker, compromising system integrity.
2. They can be directed to execute original code that triggers a preliminary failure, leading to system misbehavior.

# Buffer overflow exploit.
There are several exploits that can be used to overwrite function pointers, with **buffer overflows** being one of the most common. These buffer overflows frequently occur due to a failure to restrict writing within array boundaries, often caused by improperly bounded loops.

For a buffer overflow to affect a function pointer, the buffer must be located in the same segment as the target function pointer. Executables typically have two types of data segments: **Data** and **BSS**. The Data segment contains all initialized global variables and constants, while the BSS segment contains all uninitialized global variables.
