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

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/50742609-dc2d-4a2e-8be8-8a425ad85851)

