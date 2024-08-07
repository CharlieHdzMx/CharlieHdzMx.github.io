---
layout: post
title: Open Close Principle
excerpt: SOLID Open Close Principle
modified: 5/07/2021, 9:00:24
tags:
  - SOLID
  - Design
comments: true
category: blog
---
The principal value of Software Systems is their ability to **change during their lifetim**e; therefore, they should be open to changes. Bad designs do not allow for easy changes, and a system is considered a bad design with respect to OCP if these changes are:

1. Difficult to implement, making the system **rigid**.
2. Easy to cause the system to collapse, rendering it **fragile**.
3. Required to propagate changes to other SW components, and those SW components need to be deployed along with the change even when they are not related to this change, making the system **immobile**.

To avoid these bad designs, developers should thrive to not modify old code as much as possible when changes are being added.

Bertrand Meyer's definition of the Open/Closed Principle (OCP) is as follows: "_Software entities should be open for extension, but closed for modification_." Software entities can be classes, modules, namespaces, functions, etc.

As an example, let's consider MechLynx SW systems, which were requested to support operations written in infix notation. The structure is as follows:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/e612890d-4312-4bfc-920b-685f481a88ee)

Following the current design of MechLynx, the changes to support RPN (Reverse Polish Notation) can be added to the logic of Infix Syntax Analyzer, creating a Syntax Analyzer component:

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2f4993e6-abdf-42b8-af20-809da6e3b458)

Adding RPN support to the existing code structure might lead to modifications across multiple responsibilities, making the code more complex and prone to errors. This violates the Open/Closed Principle (OCP), as the system is not closed for modification. The OCP suggests that a system should be open for extension but closed for modification. To adhere to OCP, it's recommended to design the system in a way that new functionalities can be added without modifying existing code. This can be achieved through abstraction and creating a clear separation of concerns.

When modifications are made to existing code, especially when adding new features, there is an increased risk of introducing errors and negatively impacting existing functionality. This can result in the need for additional testing and validation efforts. Adhering to the Open/Closed Principle (OCP) helps mitigate these risks by structuring the code in a way that allows for easy extension without modifying existing, working code. This promotes a more **maintainable** and **robust** system over time.

f MechLynx is resistant to changes and modifications are causing ripple effects throughout the system, it's not fully embracing the Open/Closed Principle. An ideal software system should be designed to be open for extension, allowing for new features to be added without modifying existing, stable code. This promotes **flexibility** and **adaptability**, which are crucial aspects of a well-designed and maintainable software system.

A viable solution to adhere to the Open/Closed Principle (OCP) is to introduce an **abstract class** named SyntaxAnalyzer. Both RPNSyntaxAnalyzer and InfixSyntaxAnalyzer can then be implemented as concrete classes that inherit from this abstract class. This design fosters a clear hierarchy and establishes a common interface for various syntax analyzers, aligning with the principle's goal of keeping the system open for extension without necessitating modifications to existing, stable code.

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/253b7c36-39f7-49bf-ae03-d0eeee5caf83)

With this approach, the Infix Syntax Analyzer undergoes minimal changes, transitioning to derive solely from SyntaxAnalyzer. The introduction of an abstract class, SyntaxAnalyzer, facilitates the seamless integration of new features—such as RPN Notation support—without impacting existing code in the Infix Syntax Analyzer.

From a class-level perspective, this implementation aligns with the **Single Responsibility Principle**. The RPN Syntax Analyzer caters specifically to the RPN Notation Expert, while the Infix Syntax Analyzer is tailored for the Infix Notation Expert. This segregation of responsibilities within each class enhances maintainability and readability, contributing to a well-structured and modular system.

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/50912cd6-deba-407a-907e-896bc455dc85)





