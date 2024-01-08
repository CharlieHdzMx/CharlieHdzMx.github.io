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
The **Stable Dependency Principle** (SDP) prescribes that software components designed for ease of modification **should not** be reliant on components intended to be challenging to change. Instead, dependencies among software components should align in the direction of stability, with more stable components serving as the foundation for those that are intended to be more dynamic.

One method to render a software component "A" resistant to change is by establishing dependencies from other software components, denoted as "Bs," upon "A." The degree of dependency increases as "Bs" rely more on additional software components, whether indirectly through "A" or directly from a first-level software component dependency. If "A" lacks dependencies on other software components, it is classified as an **independent software component** by definition.

Stability of a SW Component is the ratio between incoming and outcoming dependencies as follows:

**_I= Fo/Fo+Fi_**

Where: 
I = Instability; I= 0 means stability, I= 1 means instability
Fo= Outcoming classes dependencies.
Fi= Incoming classes dependencies.

Taking the Inference Kernel from the following diagram:

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/518a6ca3-8229-4d87-807e-a37ee2a614f0)

In this scenario, three SE Presenter classes and two Compiler classes depend on Inference Kernel, while Inference Kernel itself relies on only one class in Workspace. The instability factor (I) of Inference Kernel, calculated as **I = 1/5**, indicates that Inference Kernel is relatively stable. This stability assessment is derived from the proportion of dependencies compared to the total number of classes in Inference Kernel.

Stability is not a universal attribute that all software components should aim to achieve. If all software components are highly stable, the system becomes resistant to change. In contrast, the **Stable** **Dependency Principle** (SDP) advocates for categorizing software components based on their volatility, with the objective of arranging them in a hierarchy that transitions from the highest rate of volatility to the highest rate of stability. This approach allows for a more flexible and adaptable system, where components with varying levels of volatility can be managed and modified effectively.

For example, the next system violates SDP since stable SW Components are depending from a volatile SW Component:

![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/3e8fedef-edf0-4662-a0af-93f8a66587b3)

One solution for this system is to use **SOLID** principles to redesign the SW Component relationship, specially using the **Dependency Inversion Principle** (DIP).

# The Stable Abstraction Principle
The **Stable Abstraction Principle** (SAP) dictates that SW Components shall be as abstract as they are stable; then **SW Components should depend in the direction of abstraction**.

The SAP (Stable Abstraction Principle) suggests a corrective design approach by emphasizing the relationship between abstractness and concreteness. Abstractness enables stable software components to be extended, providing a foundation for building upon existing, well-established structures. On the other hand, concreteness, as the opposite of abstractness, facilitates ease of modification for unstable software components. This principle highlights the importance of balancing abstract and concrete elements in the design to achieve both stability and adaptability within the software system.

Abstractness ratio for a SW Component can be calculated by the following formula:

**_A= Na/Nc_**

Where:
A = Abstractness; A= 0 means full concreteness, A= 1 means full abstractness
Na= Number of abstract and interfaces class within the SW Component
Nc= Number of classes in the SW Component.

For example, if Inference Kernel contains 6 classes which 2 are interfaces, then A = 1/3, meaning that is relatively concrete.

# Main Sequence
The **main sequence** aids in the design of software components by guiding the determination of their required levels of abstractness and stability. It serves as a framework to establish a balance between the need for abstraction, which allows for extensibility in stable components, and concreteness, which facilitates the ease of modification in less stable components. 

![06](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/50db6a66-9c17-436a-965a-96c4965eb51c)

The "**Too Rigid**" scenario occurs when a software component is characterized by high stability **(I = 0)** and high concreteness **(A = 0)**. This state is undesirable as it signifies a component that is overly inflexible and resistant to change. In this condition, the software component cannot be easily extended, making it challenging to introduce modifications. Therefore, it is not an optimal state for a software component to persist in.

The "Useless" scenario occurs when a software component exhibits high instability **(I = 1)** and high abstractness **(A = 1)**. This state is undesirable as it signifies a component that is overly abstract and unstable, leading to unused code. In this condition, the software component is abstract but lacks utilization by other components, rendering it ineffective and contributing to unnecessary code. Therefore, it is not an optimal state for a software component to persist in.

The Main Sequence is represented by the line that traverses from high instability and high abstractness to low instability and low abstractness. This sequence delineates the ideal range where software components should aim to position themselves. In this zone, the interplay between stability and abstraction does not result in adverse effects on the relationship between the software component and others. This equilibrium facilitates a well-designed system, ensuring that each software component aligns appropriately in terms of stability and abstraction, contributing to overall system integrity and flexibility.
