---
layout: post
title: Single Responsability Principle
excerpt: SRP Design
modified: 5/07/2021, 9:00:24
tags:
  - Design Principles
  - SOLID
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

Within a software component, a class should strive to have a single responsibility to avoid several issues:
1. **Heavy Dependencies:** Having a single responsibility avoids unnecessary dependencies on libraries that are only needed by one responsibility but not the others.
2. **Interference between Responsibilities:** Keeping responsibilities separate ensures that changes demanded by one responsibility won't inadvertently affect the other responsibilities.
3. **Minimized Impact of Changes:** Changes prompted by one responsibility won't force unnecessary rebuilding, retesting, and redeployment that could affect unrelated responsibilities.

The following image shows a class that violates the SRP:
![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/ab316e59-6f4b-4b5e-9ee6-ceae8db2f11c)

The design of this software system introduces a challenge where the `InferenceKernel` class has two distinct responsibilities: publishing results to the `SEView` and obtaining rules to infer from the `KnowledgeBase`. 
Because of its responsibility for SE View, the Inference Kernel is compelled to incorporate QWidget features, resulting in dependencies on this aspect of the Qt Framework. This leads to extended compilation and deployment times. Since the QWidget dependency is unnecessary for the Knowledge Base features, it adds an overhead for every change that the Knowledge Base may require.

For instance, if a change in the Knowledge Base indirectly affects the functionality used by SE View, it can lead to unexpected issues or necessitate retesting of SE View features. This results in increased effort for testing and developing solutions. When a change is made in SE View, it becomes necessary to build features of the Inference Kernel that belong to the Knowledge Base. Additionally, the developer must ensure that all tests related to the Knowledge Base are passing, and the Knowledge Base source code needs to be redeployed.

SRP dictates that the Inference Kernel should be split into separate classes, each with only one responsibility. One set of classes would serve the Knowledge Base, while another set of classes would serve SE View. The following image illustrates a possible approach to comply with SRP:
![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/86d0f563-b658-495e-bafb-45c9be7bac38)

This new software system design splits the knowledge base features into a class called Workspace, and the SE View features into the Inference Kernel class. In this way, changes from the Inference Kernel class does not affect the Workspace class.

Changes in one class are not going to affect the other class; for example, changes to the Inference Kernel are not going to affect Workspace. There are no unnecessary dependencies between classes. The dependency from the Inference Kernel is moved to an external interface, and the Workspace class is not required to include these dependencies during its development, testing, maintenance, and deployment.

# Source Code level SRP
The principle similar to SRP for functions is often expressed as: "_A function should do only one thing at a time_." Functions tend to grow in complexity over time, making maintenance, development, and testing more challenging to read and change.

Many Integrated Development Environments (IDEs) include functionality to extract functions from specific instructions. The recommendation is to extract functions until they are not more than 5 lines of code per function. However, this guideline can vary depending on other circumstances, such as the presence of large switch statements that make the logic clearer, the avoidance of passing too many arguments, and avoiding the return of overly complex objects, among other factors. Large functions often conceal class implementation details, where numerous local variables can be transformed into private variables, instructions can be converted into functions, and arguments and return values can be encapsulated within interfaces. Refactoring large functions in this way enhances code modularity, readability, and maintainability. 

Well-named functions are crucial for maintaining readable and understandable code. They serve as documentation and provide clear indications of the purpose of each function. Combining the extraction of functions with meaningful names enhances code readability, making it easier for developers to comprehend the functionality and purpose of different parts of the code.


