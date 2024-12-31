---
layout: post
title: Sw Architecture Design
excerpt: "Sw Architecture Design"
modified: 01/01/2025, 9:00:24
tags: [Architecture, C]
comments: true
category: blog
---

# Introduction
Architecture should not be defined as taking into account the details of implementation, such as what type of database is used, the server that will be employed, or the specific communication protocols that may be utilized. Architecture is more about how the components of the software system are defined and being able to organize them in such a way that they do not cause significant problems in cohesion, coupling, and dependencies among them during the phases of development, deployment, operation change, and maintainability. 

The general purpose of architecture is to create a schema and structure for the software system to recognize the most essential and important elements while deferring decisions on less relevant details until later, when more information is available. By postponing those details, it is possible to have time and better resources to make more informed decisions regarding the finer details of the system.

La arquitectura debe de soportar:
1. Los casos de uso y functional operation del sistema
2. La manteniabilidad del sistema.
3. El desarrollo del sistema.
4. El deployment del sistema.

## Use case and Functionality
Architecture must demonstrate that the use cases and intended functionality of the system are fulfilled at a high level. This means that the architecture should clearly display all use cases, illustrating the actors and the relationships between the components of each use case. It should visibly meet all requirements and be capable of accommodating new features or changes in requirements. If the architecture does not effectively represent the use cases, it becomes useless; it may be well-designed for maintainability and development, but it will ultimately fail to meet the expectations of clients and stakeholders.

## Operation
Architecture must also be capable of identifying resource limits and the necessary performance to meet the system's demands based on its use cases. For example, it should determine the maximum request limit that could occur and identify the resources needed to ensure the system continues to function if that limit is reached. Additionally, considerations such as allowed energy consumption, memory usage, or the supported baud rate must be taken into account. By considering these factors, it can also be determined whether there is a need for a method to extend system resources through upgrades like multiple processors, external memory, or threads.
