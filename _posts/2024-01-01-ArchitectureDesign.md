---
layout: post
title: Sw Architecture Design
excerpt: Sw Architecture Design
modified: 1/01/2025, 9:00:24
tags: [Architecture, C]
comments: true
category: blog
---

# Introduction
Architecture should not be defined as taking into account the details of implementation, such as what type of database is used, the server that will be employed, or the specific communication protocols that may be utilized. Architecture is more about **how the components of the software system are defined and being able to organize them** in such a way that they do not cause significant problems in **cohesion**, **coupling**, and **dependencies** among them during the phases of **development, deployment, operation change, and maintainability**. 

The general purpose of architecture is to create a schema and structure for the software system to recognize the **most essential and important elements while deferring decisions on less relevant details** until later, when more information is available. By postponing those details, it is possible to have time and better resources to make more informed decisions regarding the finer details of the system.

The architecture must support:
  1. The **use cases** and functional operation of the system
  2. The **maintainability** of the system
  3. The **development** of the system
  4. The **deployment** of the system

## Use case and Functionality
Architecture must demonstrate that the **use cases and intended functionality of the system are fulfilled at a high leve**l. This means that the architecture should clearly display all use cases, illustrating the actors and the relationships between the components of each use case. It should visibly meet all requirements and be capable of accommodating new features or changes in requirements. If the architecture does not effectively represent the use cases, it becomes useless; it may be well-designed for maintainability and development, but it will ultimately fail to meet the expectations of clients and stakeholders.

## Operation
Architecture must also be capable of identifying **resource limits and the necessary performance to meet the system's demands based on its use cases**. For example, it should determine the maximum request limit that could occur and identify the resources needed to ensure the system continues to function if that limit is reached. Additionally, considerations such as allowed energy consumption, memory usage, or the supported baud rate must be taken into account. By considering these factors, it can also be determined whether there is a need for a method to extend system resources through upgrades like multiple processors, external memory, or threads.

## Development
Architecture should also be defined based on the structure of the software team. For small software teams, where development and deployment can be straightforward, creating an architecture with sophisticated interfaces and dependency boundaries might cause more problems than solutions. However, for large development teams, **defining complex system structures to minimize dependencies between developers and reduce conflicts in functional changes becomes a good architectural approach**. It's also important to consider whether the software team will grow or shrink over time, and the architecture should be able to adapt to or plan for such changes throughout the project's lifecycle.

## Deployment
Architecture should **minimize reliance on numerous configuration files and complex deployment steps after the build process**. The deployment should be as straightforward as possible, with any necessary variations being easy to understand and implement. For instance, if configuration settings are stored in a file, there should be a clear process for inputting these settings, along with a reliable verification mechanism. The inputs and outputs of this process should be intuitive and user-friendly. While deployment variations and methods may be inevitable, the architecture should aim to simplify the software's interactions with these elements. The goal is to reduce dependencies and streamline the deployment process, making it more manageable and less error-prone. This approach ensures that the architecture supports efficient and reliable software deployment, even when accommodating different deployment scenarios or configuration requirements.

# Architecture Principles
To fulfill these expectations, architectural principles can be applied, drawing from *SOLID** principles ([Single Responsibility](https://charliehdzmx.github.io/blog/Single_Responsability_Design_Principle), [Open-Closed](https://charliehdzmx.github.io/blog/Open_Close_Principle/), [Liskov Substitution](https://charliehdzmx.github.io/blog/Liskov_Substitution_Principle/), [Interface Segregation](https://charliehdzmx.github.io/blog/SW_Component_Coupling/), and [Dependency Inversion](https://charliehdzmx.github.io/blog/Dependency_Inversion_Principle/)) along with other established design patterns, architects can develop systems that are flexible, modular, and easier to evolve over time.

## Layers
The definition of layers in software architecture is based on **drawing horizontal lines across the system**. The **highest layers are those closest to the most prominent business rules** or use cases identifiable by stakeholders/clients, with the application typically at the top. As you move downwards, there are layers of abstraction based on components with common interface types between horizontal layers. For example, below the application layer, there should be a component layer where interfaces can be defined as services and behave similarly. Intermediate layers can be more concrete abstractions and act as middleware between the service layer and the device abstraction layer. At the l**owest levels are components where interfaces directly interact with low-level devices or hardware**, such as drivers, I/O, communication buses, etc. 

A notable example of this layered approach is the AUTOSAR architecture, which clearly demonstrates horizontal layers following the pattern described above.

![Autosar Architecture](https://www.embitel.com/wp-content/uploads/1-AUTOSAR-Archtecture.jpg)

## Use Case

The definition of use cases is based on **drawing vertical lines across the system architecture**. These vertical lines represent unique use cases, spanning from the top to the bottom of the architecture description. Combined with horizontal layer lines, they help isolate and organize components, making them easier to identify. These vertical lines, often referred to as "stacks," group components based on their functionality within the software system. For example, in Ethernet communication, the Ethernet stack starts with the low-level driver component, followed by the middleware interface for Ethernet, then moves to the service layer to handle request dispatches for Ethernet communication, and finally reaches the application layer use case. The primary purpose of this Ethernet stack is to ensure proper communication via the Ethernet protocol, focusing exclusively on components relevant to this functionality. 

As shown in the image below, vertical lines represent these components (with only one horizontal line connecting to the Network Management component, which is irrelevant in this example). 
![Ethernet Stack](https://media.licdn.com/dms/image/v2/D5612AQGbYoVrPbkQpw/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1691900700375?e=1740009600&v=beta&t=mjthieVT3sbEB6xLsRFR5t5kT1njqTI9dNWTSFybOXk)

## Software Modes

This decoupling depends on factors such as inputs, the environment, the server, or the GUI used for deployment. For instance, the architecture may need to separate servers with higher resource demands and faster performance from those with lower resource requirements. This principle applies to any configuration or paradigm at the operational level. 

# Business Rules as pillar
Business rules are independent of any GUI, tool, database, or other components represented in the architecture. This is because business rules form the backbone of the software's purpose or intent, while the GUI serves as a means to present that backbone, and the database functions as a tool to store and retrieve data for it. Consequently, a well-designed architecture will always show dependencies leading to the business rules but never from the business rules to other components. For example, the GUI and the database _depend_ on the business rules to function correctly, and without the implementation of these rules, neither the GUI nor the database can operate as intended.






