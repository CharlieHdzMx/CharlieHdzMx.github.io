---
layout: post
title: Single Responsability Principle
excerpt: SRP Design
modified: 5/07/2021, 9:00:24
tags:
  - Design Principles
  - Cpp
comments: true
category: blog
---
The **SOLID** principles, introduced by Robert C. Martin around 2000, serve as "guidelines" rather than strict rules. Their application may not be feasible or recommended for certain software systems. The primary goal of these principles is to facilitate the creation of reusable software modules that can withstand changes over time and are easy to understand.

The design of a software system should be robust from various perspectives, including software architecture, software component design, and source code. A well-designed software system is built on two fundamental principles: **it should function correctly**, and **it should be easy to modify**. The emphasis on ease of modification takes precedence over the system's correctness. However, this flexibility should be balanced to allow, for instance, a legacy source code that works effectively to remain unchanged, ensuring that the software system does not necessitate further alterations at the architectural or software component levels.

It's worth noting that some software systems may exhibit a suboptimal architecture but possess a well-designed source code. In such cases, it's essential to recognize that architecture plays a crucial role in deployment, maintenance, and the ongoing development of the software system. While architecture may not have an immediate impact on the functionality, it significantly influences the system's long-term viability and adaptability.

# Common Closure Principle- Architecture level SRP
SRP for an architecture level dictates: “_A specific SW Component for each layer should be responsible for only and only one, reason to change (actor)_”. 

Architecture level SRP is called **Common Closure Principle** (CCP) which can be stated from a different perspective “Every class that changes for a specific reason should be grouped to a specific SW Component”. Layer of SW Components can be defined for example, as the Business Rules layer, or the Utility layer.

The following figure illustrates a software system that violed CPP.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/c57396ee-4aa1-4f89-b4bb-a75f134f7d3a)

In the described system, a software component is **serving two actors**. This introduces a potential challenge: a change requested by one actor may impact the functionality intended for the other actor. For instance, if the Actor Expert requests a modification, it could inadvertently affect the functionality designed for the Asker actor. This scenario highlights a broader issue that arises when a software component is burdened with multiple responsibilities. The consequences of such changes and interactions between different actors will be explored in more detail in the following chapter.

If the previous software system is modularized into software components based on layers (such as Business Layer and Utilities Layer), and each software component encapsulates classes and modules that change for only one reason, the MechLynx software system would not violate the Common Closure Principle (CCP). This approach ensures that each component is closed for modification only for the changes related to its specific responsibility or reason for change. The following image illustrates the result of this modularization strategy, maintaining adherence to CCP.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/cd3aac08-a8af-4836-a63e-1f1ca3be3031)

Notice that in this diagram, no SW Component depends on Inference Kernel, this is due it is an illustration of the Dependency Inversion Principle.

The Inference Kernel is the primary software component that houses the business rules. It takes data from the Knowledge Base and processes it according to the specified requirements. The PropCompiler is a stable software component responsible for compiling knowledge base sentences. It utilizes the PropCompiler Interface to deliver results to both the SE View and the Inference Kernel. The SE View represents the graphical user interface (GUI) that interacts with the Asker.

By adopting this approach, developers can clearly identify which software component needs to be modified based on the specific actor's request for change. This makes the development, deployment, and maintenance of the system more manageable and efficient.

In reality, changes from a specific actor can indeed impact various software components. However, following the Common Closure Principle (CCP) helps the software team minimize the number of software components changed by actor requests. This approach encourages a design where a software component contains classes that are modified primarily in response to requests from a single actor.

# SW Component level SRP
SRP for a SW Component level dictates: “_An class should have only one reason to change_”.
