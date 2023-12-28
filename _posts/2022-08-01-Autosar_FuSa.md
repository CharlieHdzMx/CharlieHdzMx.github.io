---
layout: post
title: Autosar Functional Safety
excerpt: Functional Safety modules in Autosar Architecture
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Functional Safety
comments: true
category: blog
---
# Autosar Functional Safety Intro
## Autosar Safe

Safety is the ability of a system to protect itself and protect users/operators from failures within the system. The system shall detect failures and behave accordingly to prevent side effects.

The **ISO 26262** defines functional safety as: "The absence of unnecessary risk due danger caused by inadequate behavior of E/E systems”.

The motivation of Functional Safety is the product manufacturing liability, this liability uses all mediums to specify the safety rate of the entity. 

Functional Safety is based on systematic failures. Systematic failures are predictable and can be corrected by changes of design, process or documentation.

From the system perspective, there are two types of failure reaction:

    1. Operational failure allows failures to stay on the ECU system with the objective of maintaining the system operating. Some rate of degradation might stay and can gradually increase on the system. This type of failure reaction is not usually used in the automotive industry.

    2. Safe Fail forces the system to enter a failure correction state to avoid any danger to the system and user. This type of failure reaction is usually used in the automotive industry.
ISO 26262

ISO 26262 is the functional safety specification used in the automotive industry. This spec indicates the functional safety development that has to be considered during the product life cycle, this product shall contain safety requirements and safety goals based on the danger analysis and assessment from HW and SW disciplines.

ISO 26262 indicates "guidelines" from different parts of the functional safety product development model as the following figure shows:
