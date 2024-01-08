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

This scenario highlights the importance of version management and compatibility considerations. In this case, the change in LCD Presenter version 1.2, which dropped support for Unicode characters, poses a potential challenge for MechLynx. Integrating the new version without awareness of this change could lead to failures in tests that were previously successful with version 1.1.

To address this, MechLynx developers need to carefully review release notes and documentation provided by the LCD Presenter team when considering an update. Understanding the changes in the new version, particularly those that may impact existing functionality, allows MechLynx developers to make informed decisions about whether to adopt the new version based on their system requirements and compatibility constraints.

**REP** is an SW Component cohesion principle since indicates that all SW entities that encloses a specific features (or are dependent from each other) should be releasable together. Classes, for example, are fine granulated SW entities, in large systems, classes are organized together for deployment and should not be versioned since this approach probably will overwhelm the release tracing system.

Versioning of releases from reuse as REP illustrates, is better that branching releases for large systems. Branching releases might involve a huge amount of effort to manage and keep on track.

# Common Closure Principle
As the **Component Cohesion principle**, CCP indicates that classes that are affected by the same change should be grouped together into a SW Component. Classes that are not affected by the same change should be separated into different SW Components.

When a modification affects a single software component, the deployment, change, and validation processes are confined to that specific component. In cases where two software entities are closely interconnected, either through a conceptual relationship or a physical dependency, it is advisable to group both entities within a single software component. Adhering to this Component Cohesion Principle (CCP) helps developers prevent the dispersion of changes, thereby minimizing the need for redeployment, revalidation, and recompilation across multiple software components.

# Common Reuse Principle
The **Common Reuse Principle** (CRP) suggests that classes which are likely to be deployed together should be organized into a software component. It emphasizes the importance of not depending on features that are not needed. This principle further implies that classes relying on each other should be grouped together, and it advocates against coupling classes that do not share closely related methods or functionalities.

# SW Component Cohesion Tension Diagram
SW Component Cohesion Tension Diagram illustrates the interplay among software component cohesion principles and their inherent conflicts, suggesting that complete coexistence is not always achievable due to the tensions between them.

While **CRP** tries to exclude classes that are not dependent from each other, **REP** and **CCP** tend to include classes that change together.

In an architecture primarily guided by the REP and CRP, changes necessitate the redeployment of multiple software components. Conversely, in an architecture centered on REP and CCP, even minor changes require new releases, leading to unnecessary releases. This underscores that these principles cannot be rigidly applied as "rules," but rather should be considered in the context of the system's current status and timeline.

As an illustration, during the initiation of projects, reusability may not be a top priority, given their nascent stage and the limited number of projects likely to derive from them. In such cases, architects should prioritize Component Cohesion Principle (CCP) and Release Reuse Equivalency Principle (REP). As projects mature, the emphasis shifts, and reusability becomes more critical. At this stage, architects should focus on both Class Responsibility Principle (CRP) and REP to ensure a balance between cohesion, responsibility, and reusability.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/81e8609a-a56a-4935-b18d-414850663ecd)
