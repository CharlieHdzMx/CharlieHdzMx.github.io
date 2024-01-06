---
layout: post
title: Liskov Substitute Principle
excerpt: LSP design
modified: 5/07/2021, 9:00:24
tags:
  - Design Patterns
  - Architecture
comments: true
category: blog
---
LSP, or the Liskov Substitution Principle, is an important feature for all software entities that conform to the Open-Closed Principle, which was introduced by Barbara Liskov. The principle is stated as follows:

_"What is wanted here is something like the following substitution property: If for each object **o1** of type **S** there is an object **o2** of type **T** such that for all programs **P** defined in terms of **T**, the behavior of **P** is unchanged when **o1** is substituted for **o2**, then **S** is a subtype of **T**."_

This means, for example, at the architectural level that all interfaces between software components should offer substitutable types. For instance:

In the MechLynx software system, there is an interface for a Syntax Analyzer that serves functions where the client can pass Syntax Analyzer references without worrying about how these references are treated. This is referred to as **extrinsic public behavior**.

The Syntax Analyzer contains two derived classes, Infix Syntax Analyzer and RPN Syntax Analyzer, both of which can behave as a syntax analyzer within their own logic. This is known as **intrinsic private behavior**.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a06b5669-5e5c-4105-98be-c325e97c77ae)

With this structure, the client (in this case, the Inference Kernel) is using the Syntax Analyzer regardless of its concrete type. Dynamically, the Syntax Analyzer will decide which type of Syntax Analyzer to use based on the specific context or conditions. This adheres to the principles of the Liskov Substitution Principle (LSP), allowing for substitutability of derived types without affecting the behavior of the client.

Now, the client has decided to add a non-mathematical notation called "Doggo Notation." Following the Open/Closed Principle (OCP), it is possible to define Doggo Notation as a subtype of Syntax Analyzer. This is because Doggo Notation contains the same intrinsic private behavior as Infix and RPN Notations. This adherence to OCP allows for extending the system without modifying existing code, supporting the principle of software entities being open for extension but closed for modification.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a39b6ef0-5b1c-4746-a595-44c4887249cd)

As Doggo Notation only interacts with Left Tokens, the Right Token interfaces are set with the value of the current Left Token. However, this implementation would be problematic: the client would expect the public behavior of Doggo Notation when passing a reference of Syntax Analyzer. Specifically, the client's extrinsic public behavior might expect that the Left Token and Right Token of all Syntax Analyzer types shall be different; this is not the case for Doggo Notation. This scenario highlights the importance of carefully designing and adhering to the principles to ensure consistent and expected behavior across different subtypes.

Since, according to the Liskov Substitution Principle (LSP), all functions that use a reference to a base class should be able to use derived classes without knowing it, some uses of Doggo Notation do not match the uses of Syntax Analyzer. Therefore, Doggo Notation violates the LSP. This emphasizes the importance of adhering to principles to maintain consistency and compatibility within a hierarchy of related classes.

A way to solve this is by considering that the relationship of real-world entities does not necessarily share the same relationship among their code representations. Therefore, Doggo Notation can be considered a subtype of Meme Notation and avoid the relationship with Syntax Analyzer, addressing the inconsistency in the code representation of these entities. This approach helps maintain a clear and consistent hierarchy, aligning with the principles of object-oriented design.

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/f854e768-98f8-4ecb-b434-c659828a8e9c)
