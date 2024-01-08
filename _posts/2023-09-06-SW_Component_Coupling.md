---
layout: post
title: SW Component Coupling
excerpt: SW Component Coupling
modified: 5/07/2021, 9:00:24
tags:
  - Software Component
  - Design Principles
comments: true
category: blog
---
In contrast to the **SOLID** principles, which guide the internal construction of a software component, the **Software Component Design Principles**, comprising both Software Component Cohesion and Software Component Coupling, dictate the organization and arrangement of software components within a larger software system. While SOLID principles focus on the internal structure of individual components, the Software Component Design Principles extend their influence to the relationships and interactions among different components in a software system.

**Software Component Cohesion** principles guide the construction of individual software components, specifying what elements should be included internally, while **Software Component Coupling** govern the relationships and interactions between these software components.

# Acyclic Dependencies Principle
The relationship between software components is contingent on their respective versions. For instance, SW Component A version 1.0 may be compatible with SW Component B version 9.2. Merely adhering to the Release Reuse Equivalency Principle (REP) is insufficient for effective package management, as it is also crucial to handle dependencies between these packages. To prevent unexpected failures, it is essential to avoid cyclic dependencies between packages. **Cyclic dependencies** can disrupt the stability of a software component that was otherwise robust, as changes in one package can inadvertently loop back and impact itself.

The initial principle of Software Component Coupling is the **Acyclic Dependencies Principle** (ADP), stipulating that "**There must be no cycles in the dependency structure of software components.**"

To illustrate ADP, consider the following diagram which shows the dependencies between packages of MechLynx.
