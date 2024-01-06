---
layout: post
title: Dependency Inversion Principle
excerpt: Dependency Inversion Principle SOLID
modified: 5/07/2021, 9:00:24
tags:
  - SOLID
  - Design
comments: true
category: blog
---
The **Dependency Inversion Principle** (DIP) suggests that a system can achieve more flexibility if its source code dependencies refer only to abstractions. While this is a valuable recommendation, it acknowledges that no software system can rely solely on abstractions, and there may be cases where stable concrete dependencies do not compromise the design. The key is to strike a balance between using abstractions for flexibility and incorporating stable concrete dependencies when necessary.

Stable elements are those that rarely change, and a software system can rely on their implementation. In contrast, volatile elements are subject to frequent changes, and relying on volatile concrete elements may be challenging since they are actively evolving.

DIP expresses that concrete implementations are generally less volatile than abstractions. This is because some changes in concrete implementations may not necessitate changes in their abstractions, while changes in abstractions will invariably correspond to changes in concrete implementations.

So, for example, when source code "_B_" depends on volatile concrete implementations "_C_," "_B_" is prompted to change with any change in "_C_." This implies that recompilation and redeployment efforts increase. In contrast, when source code "_B_" depends on a stable abstraction "_A_," "_B_" is not tied to changes in "_A_" (which might be less probable than "_C_" changes). This implies that recompilation and redeployment efforts are kept to a minimum.

In the MechLynx SW system, SW View depends on the PROPCompiler, which depends on the Inference Kernel. Thereby, any change on Inference Kernel will affect SW View.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/bc923543-c359-4912-aad0-cf8770a1fd0c)

To avoid this, the dependency on PROPCompiler should be inverted by introducing a new interface. This new interface defines a Factory class that provides instances of different types of compilers. For example, if changes in the Inference Kernel imply the creation of a new type of compiler, such as the Dogo Compiler, this interface will create an instance of the new Dogo Compiler type, and these changes will not affect the compilation of SE View.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9f71cf32-7970-4027-9333-539a51418c07)

Notice the type of the arrows; black arrows indicate a "uses" relationship, while white arrows indicate an "is a" relationship. In this case, SE View uses Compiler, and Dogo Compiler and PROPCompiler are both types of Compiler.

The runtime dependency of SE View to Inference Kernel is still there, the one that is avoided with this implementation is the compilation dependency.

The conclusion is that DIP helps us to define layers of SW Components that are independent between each other by the use of abstraction.
