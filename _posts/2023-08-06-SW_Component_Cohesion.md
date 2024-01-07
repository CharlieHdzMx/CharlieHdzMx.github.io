---
layout: post
title: SW Component Cohesion
excerpt: SW Component Cohesion
modified: 5/07/2021, 9:00:24
tags:
  - Design
  - Cpp
comments: true
category: blog
---
While SOLID principles focus on the internal design of individual software components, the Software Component Design Principles address how these components should be organized and interact within a broader software system. These principles, encompassing cohesion and coupling, play a crucial role in shaping the architecture and overall structure of the software, ensuring it is modular, maintainable, and scalable.

**Cohesion** emphasizes the degree to which elements within a component are related, while coupling addresses the dependencies between different components. Balancing these principles contributes to the creation of robust and adaptable software systems.


# Release/Reuse Equivalency Principle
As Component Cohesion principle, REP indicates that the **granule of reuse is the granule of release**, **Components should be part of a tracking system in order to be reused**.
