---
layout: post
title: Autosar Services
excerpt: Autosar Services
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Automotive
comments: true
category: blog
---
The system services cluster offers a standardized operating system and ECUspecific services. For example, watchdog manager, OS manager, Timers, Errors, and ECU states.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/480afb3a-2ab8-416c-8c15-d7da1053d681)

System services are modules that are accessible to be used by other modules from BSW. These system services can be independent or dependent on the capabilities and characteristics of the Hw. For example, Autosar OS requires specific access to microcontroller resources such as interruption control registers, stack pointers, or memory protection.

System services can be also partially dependent on the HW and the ASW, for example, ECU State Manager (EcuM) would depend on the processing of a wake-up event that comes from the microcontroller but also depends on the command of how to treat this kind of events from ASW.

# AutoSar OS
Autosar-OS can be separated into different groups according to its characteristics: statically configurable or dynamically managed. The basic features of an AutosarOS are: 
1. It is configurable and statically scalable.
2. Responds to reasoning with real-time performance levels.
3. Provides priority-based scheduling policies.
4. Provides protective functions at runtime.

AutosarOS is based on OSEK OS. OSEK OS has been an event-based operating system widely used in modern vehicle ECUs. Since OSEK OS is used also on legacy systems, Autosar can replace OSEK OS on such systems. AutosarOS uses dynamic schedules to process actions at a specific time, this specific time is called the expiry point and serves to correctly synchronize the schedule. AutosarOS must also protect the memory and stack from ISR/Tasks that exceed the use of these resources. Due to circumstances that independent task monitoring to detect faults may be inconclusive, it is recommended to let the OS identify which Task or ISR has the error. This is a common approach when the current task is not consuming these resources and it is getting the blame from another SW entity that is preempted and consuming the resources on hold.

## OS Application
OS Applications are objects that form a functional unit of OS. OS applications are Tasks, ISRs, Alarms, Schedule tables, and counters. The OS manages the use of resources among existing OS applications. Resource-use depends on the type of OS application, when the OS application is “trusted”, it may have unrestricted access memory and unforced time in runtime. On the other hand, when the OS application is “Non-Trusted”, it is not allowed to be executed without monitoring and protection, in other words, the “Non-Trusted” OS Application has memory and runtime restrictions.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/8f244cee-c0f6-45ee-960e-daf55752e573)

## OS Protection Methods
Autosar OS prevents the propagation of timing failures in Task / ISR sequences using **Budgets** and **Times** Frames:
1. For Task / ISR execution failures, an **Execution Budget** is used.
2. For blocking of resources shared by a lower priority Task / ISR, a **Lock Budget** is used.
3. For interlocking Task / ISR ranges, a **Time Frame** is used.

This Time Frame controls the time between successive activations of Tasks instances, even though this task is already in the READY / RUNNING state. Protection of services is necessary to ensure that interaction between OS-application and Autosar OS will not corrupt the OS, for example, leaving the OS in an undefined state or incorrectly handling out-of-range values or actions of other OS-Applications. Service protections can be service-blocking in specific circumstances, API usage methodologies, and Non-Trusted OS-Application restrictions.

## ECU State Manager
The ECU State manager (**EcuM**) manages the ECU states. EcuM handles Wake Up, Sleep, and Shutdown states of the ECU; as well as the initialization and de-initialization of the OS, Schedule Manager, and BswM.

EcuM can be a flexible ECU Manager that allows, among other things, having a partial startup, giving priority to early initialization of SWCs or partitions. EcuM can also have multiple operating states and different multicore states.

## Diagnostic Event Manager
Diagnostic Event Manager (**DEM**) processes diagnostic events reported from BSW or SWCs. DEM can request NVRAM Manager to persistently save fault data and serves as a mediator for fault data from DTCs for Diagnostic Communication Manager (**DCM**) and functional degradation strategies for FIM.

DEM has a high correlation with OEM requirements based on diagnostic use.

DEM uses atomic units to represent the diagnostic results of monitoring in SWCs. These atomic units are known as Diagnostic Events, DEM uses IDs to manage the status of these Diagnostic Events and perform specific actions. Diagnostic Events have priority and an occurrence counter. Diagnostic monitoring is a routine that determines the correct operation of an SWC. When a specific failure occurs in a physical system, the diagnostic monitor links a Diagnostic Event. There are diagnostic events dedicated to each failure for each physical system.

A Diagnostic Trouble Code (**DTC**) defines a unique DEM identifier related to a Diagnostic Event. DEM provides DCs status for DCM, so allows that DCM communicates DTCs properly (whether OBD or not) to external testers.

## Function Inhibition Manager
Function Inhibition Manager (**FiM**) calculates the inhibition conditions and permissions of the functionality of an SWC. FiM inhibitions are highly related to the status of a diagnostic event from DEM because failures are the main factors of inhibition of functionalities.

Each function has assigned an identifier (**FID**) containing inhibition conditions. Before a function is executed, FiM checks the conditions through the FID, if there is a condition that returns Inhibition (True), then FiM does not allow the execution of such functionality.

## Default Error Tracer
The Default Error Tracer (**DET**) provides interfaces to report development errors. DET APIs are defined by Autosar, unlike the full implementation and configuration of DET related functionalities.

DET checks BSW / SWCs for wrong parameter values, buffer overflow, or poor call sequence. This is done through error counters, configurable buffers that save development errors, logging mode with breakpoints, notification of callbacks, and error hooks. Error hooks are configurable report functionalities for received logged errors.

## Time Service
Time Service (**Tm**) provides services for time-based functionalities such as time measurement, state machines with debouncing or timeouts, busy/waiting states. TM contains specific timer types, each timer type has a predefined tick duration and a bit range number, with these two features, TM ensures that every platform supports some timer type. Each Timer type is based on GPTs that are free run HW timers.

## Synchronized Time-based Manager
The synchronized based Time Manager (**Stbm**) provides synchronized time bases with other modules of a distributed system ECUs. This is useful when Runnables entities require synchronized execution or SWC / BSW requires absolute times for recording and accessing event data in real-time.

## Watchdog Manager
Code execution failures (or timing failures) can vary as follows:
1. Code executed before or after expected
2. Code executed without request.
3. Code that execution is too long.
4. The code flow does not match what was expected.

The ways that Watchdog deals with these failures:
1. **Deadline Monitoring**, applied in aperiodic execution, compares the time between 2 checkpoints delimiting maximums and minimums.
2. **Alive Monitoring**, applied in periodic executions, checkpoints are monitored in a specific time interval.
3. **Logic Monitoring**, applied in incorrect execution flows, validates the sequence of checkpoints against a predefined sequence.
   
The watchdog manager (**WdgM**) can detect timing failures in both application and BSW. When detecting a timing failure of a supervised SW entity, the WdgM will command the reset the ECU with optative actions. Reporting of inconsistencies in checkpoints from a Mixed ASIL ECU can be inherited between SWCs of different ASIL level, this is done so as not to have to change the OS context.

WdgM monitors the supervised entities, when a supervised entity has any execution timing problem, WdgM returns a value of NOK and causes an ECU reset, usually. A Supervised Entity (SE), is an abstract SW entity that is monitored by WdgM. A SE contains 4 states that allow WdgM to know if it has a timing execution failure. The main states that an SE can have are Ok, Deactivated, Expired, and Failed. The difference between expired and failed is that “failed” can return to “ok” using debounce strategies while being expired, it is no longer possible to recover.

A **checkpoint** (CP) is an SW target within the logic flow of an SE. Checkpoints notify WdgM when SE logic reaches a key point. Checkpoints can be called directly with a method _CheckpointReached_() or indirectly by RTE. CPs are part of a single SE, this means that a CP cannot be used in different SEs. In an SE, there must be at least one Global Initial CP and optionally several Global End CPs.

**Transitions** are references flow between Checkpoints, transitions can be global (transitions between SEs checkpoints) and transitions can be local (transitions within a checkpoint). Communication Manager
Communication Manager (ComM) controls the BSW modules related to communications by collecting and coordinating communication access requests from buses. ComM simplifies request processing at a high level by coordinating communication requests based on the availability of the communication stack and the hosting of communication resources.

## Basic SW Mode Manager
Basic SW Mode Manager (**BswM**) is the main responsible for managing BSW modes. When an SWC requests a BswM mode change, BswM handles that request even when it references to some other ECU. When BswM had finished performing “actions” triggered by this request, it returns a result via RTE to all involucrated SWCs to notify that they can be changed to the way they wanted.

# Multicore System Services
Inter-OS Application communication (**IOC**) provides communication of services that can be accessed between cores in the same ECU. That is communication through OS-Applications boundaries and memory partitions. When BSW is separated into different cores, the core through IOC and the ECU state manager will be responsible for executing services in runtime. that supports communication with IOC, even when it is possible for Complex Drivers to access it through extra configuration.

IOC provides only Sender / Receiver communication with support for atomic non-queued communication (1: 1), N: 1 and N: M. Communication between OS applications in multicore is done by RTE. RTE is the only one
![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/50742609-dc2d-4a2e-8be8-8a425ad85851)

