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
