---
layout: post
title: Interface Segregation Principle
excerpt: ISP
modified: 5/07/2021, 9:00:24
tags:
  - Design Principles
  - Cpp
  - Interface
comments: true
category: blog
---**
The **Interface Segregation Principl**e (ISP) is particularly relevant in **statically typed languages where changes to one set of features should not force the recompilation and redeployment of unrelated source code. In dynamically typed languages, such as Python, PHP, Ruby, etc., dependencies are resolved at runtime, and there is no need for explicit header declarations, making the ISP less applicable.

By adhering to ISP in statically typed languages, developers can create fine-grained interfaces that cater to specific client needs, preventing unnecessary recompilation and deployment of unrelated code components. This modular approach enhances maintainability and flexibility in large codebases.

ISP prevents unnecessary dependencies on modules that contain features not required by a particular client. For example, in MechLynx, there is a Conclusion Widget responsible for listing various diagnostics inferred by the system based on user input. The interface used to set a conclusion might look like this:
