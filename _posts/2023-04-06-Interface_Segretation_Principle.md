---
layout: post
title: Interface Segregation Principle
excerpt: ISP
modified: 5/07/2021, 9:00:24
tags:
  - Design Principles
  - Cpp
  - SOLID
comments: true
category: blog
---**
The **Interface Segregation Principl**e (ISP) is particularly relevant in **statically typed languages where changes to one set of features should not force the recompilation and redeployment of unrelated source code. In dynamically typed languages, such as Python, PHP, Ruby, etc., dependencies are resolved at runtime, and there is no need for explicit header declarations, making the ISP less applicable.

By adhering to ISP in statically typed languages, developers can create fine-grained interfaces that cater to specific client needs, preventing unnecessary recompilation and deployment of unrelated code components. This modular approach enhances maintainability and flexibility in large codebases.

ISP prevents unnecessary dependencies on modules that contain features not required by a particular client. For example, in MechLynx, there is a Conclusion Widget responsible for listing various diagnostics inferred by the system based on user input. The interface used to set a conclusion might look like this:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a50e7968-de7f-42d6-8534-835be85c0a1b)

But the system has different conclusion types, such as Transmission conclusion, Engine conclusion, or Brakes conclusion. All these software entities will use the `setItem()` function.

Certainly, the `ConclusionSetter` class is doing only one thing: setting conclusions. However, the principal problem with this approach is that if any of these conclusions want to change, the logic for all of them can be affected. If, for example, the `EngineConclusion` requires following a hyperlink instead of uploading an icon, then this change is not required by the other conclusion types, but they will be chained to be compiled and be deployed with a feature that they don’t need.

A better approach is to "segregate" the customized features for each conclusion type to be allocated to a personalized interface, later to allocate the main processing to the class `ConclusionSetter`. By this way, this design complies with ISP:

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9973c99d-2267-4fc9-826a-43a2b62e0675)
