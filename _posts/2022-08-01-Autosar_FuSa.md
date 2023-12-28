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

1. **Operational failure** allows failures to stay on the ECU system with the objective of maintaining the system operating. Some rate of degradation might stay and can gradually increase on the system. This type of failure reaction is not usually used in the automotive industry.

2. **Safe Fail** forces the system to enter a failure correction state to avoid any danger to the system and user. This type of failure reaction is usually used in the automotive industry.
## ISO 26262

ISO 26262 is the functional safety specification used in the automotive industry. This spec indicates the functional safety development that has to be considered during the product life cycle, this product shall contain safety requirements and safety goals based on the danger analysis and assessment from HW and SW disciplines.

ISO 26262 indicates "guidelines" from different parts of the functional safety product development model as the following figure shows:

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/3f48d694-51e9-414c-a3c0-25a2d70f593a)

## Concept Phase

There are different functional safety workproducts that are defined based on the collaboration between the OEM, and the Tier1/Tier2. The Functional Safety work product responsibilities are as follows:

1. OEM is responsible to define Item definition, Risk and danger analysis and assessment.

2. OEM with Tier1/Tier2 are responsible to define Functional Safety Concept, Technical Safety Concept, Safety HW Requirements and SW Safety Requirements.

**Item definition** is the definition of what is an "item" and its features; it is the interdependent relation between the item and the environment.

The **Risk and danger analysis and assessment** identifies and collects the possible scenes of item failure to determine the potential risk. The collection is based on the **severity** ( damage caused by the failure), **exposition** (how frequent or prolonged is the failure) and the **controllability** (how easy is to exit the failure). The collection of these factors are evaluated based on levels, for example, Severity rate is from S0 to S3 being S0 "no damage" and S3 "death probability". This evaluation from each scene allows the determination of ASIL rate and also allows to determine the safety goals. The lowest ASIL rate is QM (Quality Management) and increases from **ASILA** to **ASILD**.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/8475f106-9fb5-40ab-af67-0dc51af4e94b)

**ASIL QM** systems do not have Safety Requirements. ASIL A, ASIL B, ASIL C and ASIL D indicates incremental rate of safety severity. Having ASILs at each possible scene of risk allows the definition of Safety Goals for each ASIL level. Eventually, the higher ASIL level is defined to represent the Safety level on the system.

**The Functional Safety Concept** takes the ASIL rate from each malfunction and the safety goals from each scene and it explains how to reach the safety goal from an architectural perspective to implementation. Functional Safety Concept also contains (at least one) Functional Safety Requirements .

The** Technical Safety Concept** takes the Functional Safety Requirements, Safety Goals and ASILs level to determine how to implement the Safety Requirements to a system. Technical Safety concept indicates that all HW Safety Requirements and SW Safety Requirements shall be defined along with their respective Safety tests.

### Safety Implementation Concept

There are two concepts of solutions of Safety implementation: Element Duplication and Addition of different elements:

1. **Element duplication** is defined as the case where an element failed and a way to gradually establish the system from a Fail Safe is by activating a similar element than the one that failed. This approach is effective only for HW elements and conceptually it cannot guarantee the inhibition of the failure, it only decrements the range of random failures.

2. The **Different Elements Addition** happens when an element failed and the way to establish the system from a fail safe is by activating a different element than the one that failed. This approach is effective on both SW and HW, and it solves both systematic and random failures.

### ASIL Decomposition

The different element addition strategy can use **ASIL decomposition**. ASIL decomposition indicates that a high ASIL level element can be decomposed into various lower ASIL level elements and still comply with the high ASIL level requirements. For example, an ASIL D element can be decomposed into two ASIL C elements and still complying with the ASIL D level requirements.
![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/08f8af9b-6c32-4959-bb92-cf054ab10ac3)

## Mixed ASIL systems

When there are two elements with different ASIL levels within a system, and this system shall ensure the higher ASIL level requirements, then one solution is to ensure that the lower ASIL level element can fulfill the higher ASIL level Safety requirements.

# Autosar Safety Software
## Freedom From Interference

Freedom From Interference (**FFI**) is the failure propagation absence that one element can produce and affect a higher ASIL level element. In other words, FFI ensures that the communication or dependency between two different ASIL level elements do not affect the safety requirements of one of them.

The effects that prevent FFI can be labeled as:

1. Timing/Execution failures, Deadlocks, execution blocking, no phased synch between SW elements.
2. Memory failures, memory corruption.
3. Information exchange.

To achieve FFI, there are different mechanisms to deal with the failures that were described above, these mechanisms can be used together:

1. **Barriers** usually are supported by HW (MPU and external watchdogs) that prevent the interference between elements.
2. **Defenses** are programming techniques and methodologies to disable interruptions during critical sessions, pointer checking mechanism and so on. Defenses are effective against memory corruption, but they are not effective against dynamic calling failures.
3. **Qualifiers** are documentation with convincing evidence that demonstrate that one element has its own mechanisms to ensure FFI.
    Well trusted elements, ensure no ASIL level corruption.

### SEooC

Safety Elements Out Of Context (SEooC) are generic SW elements that have no defined use case in the system, but can be integrated to the system. This integration depends on whether or not SEooCs has evidence or assumptions that are compliant with the Safety Requirements of the system.
## BSW Safe Modules

Autosar vendors such as Elektrobit and Vector have their own Safe implementation within an Autosar architecture.

For example, Vector Microsar is a embedded software which contains the following safety modules:

1. **SafeOS**. Support memory partition and MPU configuration. SafeOS allows safe context switching for single/multiple cores.
2. **SafeE2E**. Ensures the safe communication among ECUs.
3. **SafeWdg**. Detects ejecution order and timing failures by providing logic monitoring and deadlines.
4. **SafeRTE**. Ensures the safe communication within an ECU.
5. **SafeBSW**. Reduces the partition change frequency and completes the BSW to comply with Safety requirements. SafeBSW is usually used for high performance integrity systems (ASIL C and ASIL D).

EB Tresos defines Safety BSW modules such as:

1. **Tresos Safety OS**, supports memory partition, MPU configuration and safe context switch among cores.
2. **Tresos Safety RTE**, ensures safe communication within the ECU.
3. **Tresos Safety Timing**, timing and execution supervision of safety-related applications.
4. **Tresos Safety E2E**, guarantees safe communication between ECUs based on protection wrappers and safe data transformations

Mixed ASIL level systems can use all Safety models excepting SafeBSW. Again, SafeBSW is used only for systems with high performance integration (high ASIL level).
## OS Context change

The changes of **OS context** can occur when a mixed ASIL level system is using both Safe BSW and non-Safe BSWM. The following figure shows a system with ASIL partitions and QM partitions:

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
















