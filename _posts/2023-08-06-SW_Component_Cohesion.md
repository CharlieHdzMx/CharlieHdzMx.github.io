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

Component Cohesion principles indicate how to build SW Components (what is put inside),while Component Coupling principles dictate the relationship between SW Components.

# Release/Reuse Equivalency Principle
As Component Cohesion principle, REP indicates that the **granule of reuse is the granule of release**, **Components should be part of a tracking system in order to be reused**.

software components play a crucial role in modularizing large projects and enhancing the maintainability of the system. They serve as deployable packages, acting as independent entities that can be attached or plugged into a system. This modular approach allows for efficient bug fixing and updates without the need to modify the entire system, promoting a more scalable and flexible software architecture.

In this context, software reuse refers to the capability of incorporating software plugins into a user's system without having to worry about the internal details of those plugins. For example, if MechLynx integrates a software plugin named "LCD_Presenter," MechLynx developers only need to focus on utilizing the public interface of the LCD Presenter correctly. They are relieved from the burden of managing bugs and features within the LCD Presenter, promoting a more streamlined and efficient development process.

MechLynx developers can select a specific "version" of the LCD Presenter that is compatible with their system and continue using this chosen version. Versions of the LCD Presenter represent release deployments provided by LCD Presenter developers to cater to the diverse user base of LCD Presenter. This versioning approach helps maintain stability and consistency in MechLynx's integration with the LCD Presenter.

As LCD Presenter evolves to enhance stability, assigning version numbers to each release is essential. This versioning practice facilitates clear communication with users by providing a systematic way to track changes and updates. Users can then make informed decisions about whether to adopt the new version based on the release notes and changes highlighted by the LCD Presenter developers.

Well-designed plugins adhere to a clear versioning strategy, ensuring that users interact solely with public interfaces. This separation allows users to leverage the plugin's functionality without being burdened by internal changes. When a new version is released, users can evaluate the release notes and assess the impact on their systems. This flexibility empowers users to make informed decisions about updating their plugin versions based on their specific needs and requirements.
