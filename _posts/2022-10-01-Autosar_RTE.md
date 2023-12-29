---
layout: post
title: Autosar RTE
excerpt: Autosar RTE
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - RTE
comments: true
category: blog
---
**Runnable** is the active part of an SWC, can be executed concurrently with the mapping of different tasks. Various tasks can be supported by one OS Application, each **OS Application** will be in part of at least one **partition**, and each partition can contain the functionality of different SWCs and uses different BSW resources. For multicore ECUs, each core can have different shared OS Applications.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/f818468c-4db3-47cf-ac75-2084746f263e)

# Partitions
Partitions are based on the use of OS-application. OS-application permits the logical grouping of SWCs and its resources. Each OS-application has its own individual policies of error recovery, for example, memory violation error recovery or time budget expiration action. Runnable of one OS-application can execute these policies of error recovery based on their own priority and type, for example, there are some  OS-application that policies of error recovery do not anything.

# Protection Hooks
Hooks are functions that are called by the OS if certain conditions are found. Protection Hooks are called when certain errors had been present, this kind of hooks can execute actions against the detailed OS-application that is failing, such as terminating the task and ISR that is causing problems, or terminating the complete OS-application, or shut down the entire ECU, or just doing anything. These protection hooks at the end, return a value to its caller notifying the result of the actions performed. When Protections Hooks return anything, the OS shall execute a predefined logic as the Protection Hook was not configured.

Due that normally a protection hook executes itself in a privileged context of the OSapplication, this Protection Hook shall be defined as “Trusted”. If one OS-application is failing, these are one possible steps that are going to be executed from the error detection to the complete recovery of the OS-application. 

1. If one error is detected on an OS-application, then this OS-application is commanded to restart without affecting the other related OS-applications; this command will trigger the Protection Hooks.
2. Protection Hooks will indicate to RTE that there is an error based on their configured error detection policies.
3. When receiving a response from RTE, the Protection Hooks will command specific actions; in this case the restart of the OS-application.
4. The OS application that presents the error is terminated, and all the communications related to this OS-application are stopped and the ports will be set with a default value.
5. The OS-application starts a recovery process by the calling of function OSRestartTask() that can occur when an action of a Protection Hook is “restart”. RTE also starts a cleanup and reset any communication related to this OS-application, while the OSRestartTask() triggers various BSW modules clean-up actions such as save important data to NVM.
6. The OS-application returns to its correct state and the real communications start again.

# Scheduler
BSW and RTE are generated together to permit as much as possible that the minimum OS tasks perform the scheduling of BSW functions and Runnable entities. This also permits the synchronization and behavior of state/mode changes of these BSW functions and Runnable entities. The modes that affect the BSW functions and Runnables require one ModeDeclarationGroupPrototype to provide APIs to communicate the actual active mode.

Each BSW module, by itself, is incapable of knowing the ECU time dependency and which mechanism is available to implement efficient consistency of data. To ensure the correct behavior and timing of the different modes of BSW and their interaction between them, the BSW schedule is centralized to a System service module called BSW Scheduler. BSW Scheduler is configured by the ECU integrator and it is generated together with the RTE in order of:

1. Embed the implementations of the BSW modules into the Autosar OS context,
2. Permit the execution of different scheduling strategies to a group of objects.
3. Activate consistency mechanism to data of a collection of objects.
4. Reduce the use of resources by general task calling that function to different entities.
5. Activate execution sequences between Runnable entities and BSW functions.

BSW scheduler creates BSW Scheduling objects to represent BSW modules in the schedule,
as well as BSW scheduler can define BSW events type such as:
1. BswTimingEvent
2. BswBackgroundEvent
3. BswModeSwitchEvent
4. BswOperationInvokedEvent

# Mode Management
Each system contains different modes with various degrees of granularity, these modes are the Vehicle Mode, Application Mode, and BSW Mode. Between these modes, there are dependencies such as:
1. One change in Vehicle Mode can cause a change in BSW mode; and vice-versa.
2. One change in the Vehicle Mode can cause a change in Application mode; and viceversa.
3. One change in BSW mode can cause a change in Application mode; and vice-versa.

The mode management is based on the distribution and arbitrary of the mode change request and BSW/ASW local results controlling. For example, depending on the mode, the application can act passively by a change of mode or request a change of mode. The application can also be kept activated or deactivated directly by the mode change; this depends on the policies of mode change of each individual runnable, due that application can have different ways to function depending on which mode is already active. 

Mode change is communicated by sender/receiver communication requests. Each OS application will have a mode manager that will change the BSW mode locally.

BswM is the principal responsibility of managing modes of BSW, this module is located on the system service layer. When an SWC asks for a mode change, RTE distributes the request of mode change before sending this request to BswM, when BswM receives this request, it propagates the request by Sender/Receiver communication running different OSApplication for each involved module, even when they are referring another ECU. When finishing the “mode change actions” BswM returns the result of mode change to RTE and later to all the involucrated SWCs.

For example, one mode change cycle can be as follow:
1. One SWC requires a mode change using its sender port.
2. RTE distributes the request to BswM.
3. BswM determines the rules of that request and executes a list of actions.
4. Some actions might ask BswM to indicate RTE the result of the mode change to all users subscribed to be informed by mode change.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/01a60f9b-4773-4196-88d1-d0051053fdc0)

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a2ccfd56-dfe3-4663-a3d3-f8ff4dd724d6)

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2e21f89c-7fc7-4f13-9e9a-99c1304f7e7a)

![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2b03146c-3b46-4ddb-afa8-06f839d2f328)

![06](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/553c0b10-eea8-42ad-bdd7-b932e90f297d)

![07](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b112ff17-a347-4074-b23e-f86d12b9dd96)


