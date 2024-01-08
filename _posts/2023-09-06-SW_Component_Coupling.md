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

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/baa7ef4c-d1fb-4b21-9f5a-c8fb5add653a)

This forms a Direct Acyclic Graph as there is no **dependency cycle** between packages. When a new version is released by SE Presenter, it impacts SE View and ASW. Inference Kernel, Compiler, and Workspace remain unaffected, resulting in no need for revalidation, redeployment, or recompilation, which is advantageous. However, consider a new requirement that establishes a dependency from SE View to Inference Kernel. This scenario is depicted in the following image:

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2861ef3e-9474-44d0-8c5a-1e6954e2fb87)

This configuration ceases to be a Directed Acyclic Graph (DAG) due to the emergence of a **cycle** involving Inference Kernel, SE View, SE Presenter, and ultimately looping back to Inference Kernel. Such cyclic dependencies can give rise to various issues, including challenges with version dependency, validation dependency, and build disorder.

Version dependency arises when packages are specifically designed to work together with particular versions. In this scenario, alterations in Inference Kernel necessitate corresponding changes in SE Presenter and SE View. Subsequently, when modifications to SE Presenter and SE View are introduced, Inference Kernel will require further adjustments to reconcile with its original version. This interdependence can create a cascading effect, leading to a cycle of changes across the interconnected packages.

Prior to this change, Inference Kernel validation was independent of any package, and the written test did not require modification. However, with the introduction of this dependency cycle, Inference Kernel is now dependent on SE Presenter and SE View. Consequently, these dependencies necessitate new rounds of revalidation. Furthermore, this revalidation becomes intricately tied to the versioning of these packages due to the introduced version dependency.

This cycle in the dependency graph makes the order of build to be blurred, this can cause difficult to solve problems for **statically typed languages** (C++, C, Java, …).

# The Stable Dependencies Principle

