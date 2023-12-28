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

The **ISO 26262** defines functional safety as: "_The absence of unnecessary risk due danger caused by inadequate behavior of E/E systems_”.

The motivation of Functional Safety is the product manufacturing liability, this liability uses all mediums to specify the safety rate of the entity. 

Functional Safety is based on systematic failures. Systematic failures are predictable and can be corrected by changes of design, process or documentation.

From the system perspective, there are two types of failure reaction:

    1. Operational failure allows failures to stay on the ECU system with the objective of maintaining the system operating. Some rate of degradation might stay and can gradually increase on the system. This type of failure reaction is not usually used in the automotive industry.

    2. Safe Fail forces the system to enter a failure correction state to avoid any danger to the system and user. This type of failure reaction is usually used in the automotive industry.
ISO 26262

ISO 26262 is the functional safety specification used in the automotive industry. This spec indicates the functional safety development that has to be considered during the product life cycle, this product shall contain safety requirements and safety goals based on the danger analysis and assessment from HW and SW disciplines.

ISO 26262 indicates "guidelines" from different parts of the functional safety product development model as the following figure shows:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/3f48d694-51e9-414c-a3c0-25a2d70f593a)

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/8475f106-9fb5-40ab-af67-0dc51af4e94b)

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/08f8af9b-6c32-4959-bb92-cf054ab10ac3)

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/27363240-79f2-4586-ad65-8c1b1936b209)

![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9260b534-3cac-49ae-82c0-3c60ee7a3728)

![06](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b91e6954-e70e-4c0b-965a-44b6bee49a4b)

![07](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/32626729-836c-4069-ac1e-f44626288870)

![08](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9cff8b45-3265-4e3e-a633-80ae766a083e)

![09](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/0c1ae6ea-6200-485c-8ef9-c6b79a809b4e)

![10](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/293dd805-6796-4e5a-9b77-f6c5fe211060)

![11](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/f47884fa-aaaf-40a7-b01c-82fadeed9fa3)

![12](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b8106eee-48c1-498d-96d2-a351bc23f403)

![13](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/16cbd571-721d-445b-9ca6-8fef8dc8a05f)

![14](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2b970f4a-e573-4e27-abf2-2bdfe537537b)
















