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
Each system contains different modes with various degrees of granularity, these modes are the** Vehicle Mode, Application Mode, and BSW Mode**. Between these modes, there are dependencies such as:
1. One change in Vehicle Mode can cause a change in BSW mode; and vice-versa.
2. One change in the Vehicle Mode can cause a change in Application mode; and viceversa.
3. One change in BSW mode can cause a change in Application mode; and vice-versa.

The mode management is based on the distribution and arbitrary of the mode change request and BSW/ASW local results controlling. For example, depending on the mode, the application can act passively by a change of mode or request a change of mode. The application can also be kept activated or deactivated directly by the mode change; this depends on the policies of mode change of each individual runnable, due that application can have different ways to function depending on which mode is already active. 

Mode change is communicated by sender/receiver communication requests. Each OS application will have a mode manager that will change the BSW mode locally.

**BswM** is the principal responsibility of managing modes of BSW, this module is located on the system service layer. When an SWC asks for a mode change, RTE distributes the request of mode change before sending this request to BswM, when BswM receives this request, it propagates the request by Sender/Receiver communication running different OSApplication for each involved module, even when they are referring another ECU. When finishing the “mode change actions” BswM returns the result of mode change to RTE and later to all the involucrated SWCs.

For example, one mode change cycle can be as follow:
1. One SWC requires a mode change using its sender port.
2. RTE distributes the request to BswM.
3. BswM determines the rules of that request and executes a list of actions.
4. Some actions might ask BswM to indicate RTE the result of the mode change to all users subscribed to be informed by mode change.

![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/01a60f9b-4773-4196-88d1-d0051053fdc0)

# Error handling, diagnosis, and report
There are three types of SW errors reporting:
1. Development Errors report: Errors that can be found during development phases, these reports can be deactivated when the product is during the production phase.
2. Runtime Errors report: Indicate severe and systematic SW errors.
3. Production Error report: are normally required to save loggings to later be used as information when the ECU system is repaired on an automotive dealer “garage”.

The different ways to report the previously mentioned errors are:
1. **APIs**. Informing the function that is calling the operational failure.
2. **Callback**. Defined statically and informing the caller about the operational failure.
3. **Central Error Hooks**. To log and track development errors.
4. **DET Central callouts**. To manage errors during the product lifetime.
5.** DEM central error function**. Used principally to error reactions and production code logging.

**Diagnostic Event Manager** (DEM) serves every SWC error reporting during development, production, and after-production cycles. These errors react specifically by the configuration definition, for example, they react by writing the errors in memory, deactivate functions from the ECU, or notifying several SWCs to let them act against the error.

**Default Error Tracer** (DET) severs to report errors of development cycles of all the levels of SW. The DET error reactions can be the error counting, writing the errors into RAM, logging the error externally, or defining breakpoints.

**Diagnostic Log and Trace** (DLT) collects all the log messages either when these messages are errors, warnings, or info messages coming from SWCs. These messages are converted into standardized formats, to later be forward the data to PDUR in order to send the data to their specific vehicle network communication protocol. Thereby, DLT logs communication messages to the SWCs too, tracking the RTE relevant activities, controlling the logs based on their pre-configured priority, and generate mechanisms of security that prevent misuse of these logs during production cycles.

**E2E** protection communication ensures the secure data exchange in run-time to detect and
handle systematic failures of lower layers than ASW. E2E prevents that these failures affect ASW. The way that E2E protect the communication is based on the different configurable and adaptive profiles used by the SWC such as:

1. Data Control addition to Functional Safety data that is leaving the SWC.
2. Verify data that is intended to enter to the Functional Safety SWCs by data control checking.
3. Throwing or allowing data that passed the data control verification, and returning the result to the appropriated SWCs.
   
![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/a2ccfd56-dfe3-4663-a3d3-f8ff4dd724d6)

Safe E2E protection resolve errors of ECU to ECU communication. Some errors that Safe E2E
supports are:
1. Lost messages.
2. Delayed messages.
3. Incorrect insertion of messages.
4. Non-expected duplication of messages.
5. Message corruption.

The principal ways that Safe E2E solves these errors is by:
1. CRC. Numeric value based on the message content to detect the corruption of this
2. Sequence counter. It counts the message order to detect message duplication or incorrect insertion of messages.
3. Timer. Inserted on the receptor side to determine if one message is lost or it is delayed.
These solutions can be grouped together into two variants: Protection Wrappers or Transformer solutions.

E2E protection wrapper (E2E PW) overwrites the traditional RTE function Rte_write and Rte_read for its own E2E PW wrapper (E2EPW_Write and E2EPW_Read). To do this, E2E PW needs the E2E and CRC libraries, and it only works for conventional I-PDUs. E2E PW is an intermediate entity between SWCs and RTE. When E2E transmission is required, PW attaches the counter and CRC to the message and forwards this message to
RTE. Otherwise, when E2E reception is required, the E2E PW takes the message and processes the CRC and counter before passing the message to the SWC. Additionally, while intermediating messages between SWC and RTE, E2E PW can report the status of the transported messages.

![04](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2e21f89c-7fc7-4f13-9e9a-99c1304f7e7a)

The RTE commands communication service clusters to permit E2E communication. These services process the information from RTE transformers. RTE transformers are responsible to serialize(complex data to byte arrays) and deserialize(byte arrays to complex data) data from/to SWCs. Transformer chains are a sequential set of transformers that are grouped together. Each transformer performs a transformation over the data and pass that transformed data to the next transformer. The principal transformers are:
1. COM-based transformer (**COMXF**): Group signal transformer type that its transmissions are connected to COM BSW module. These group signals are transmitted by Sender/Receiver PDU interface type with a constant length and byte interpretation.
2. Some/IP transformer (**SOME/IPXF**): Can be transmitted by S/R and C/S interfaces, this transformer performs serializations and deserializations to communicate with LDCOM (Large Data COM).
3. E2E Transformer (**E2EXF**): use Client/Server communication, both queued or unqueued. It is only used between cyclic communication. E2EXF replaces E2EPW APIs.

![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2b03146c-3b46-4ddb-afa8-06f839d2f328)

# Calibration and measurement

XCP is a standardized calibration for ECUs. XCP helps the synchronization of data acquisition and stimulus of memory pages and reflashing of ECU during development cycles. 

# Runtime CyberSecurity SecOC
Security on On-board Communication (SecOC) is an additional module at the Autosar communication stack related to PDU transmission authentication and integrity. SecOC retrieves PDUs from PDUR and adds security information to encrypt based on the PDU payload and the SOME/IP transformer use. SecOC supports various communication buses such as CAN; FlexRay and Ethernet but is not supporting LIN yet.

![06](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/553c0b10-eea8-42ad-bdd7-b932e90f297d)

One example of how is the interaction of SecOC with other modules is:
1. PDUR receives one PDU from one of the different vehicle network communication channels (CAN, FlexRay, Ethernet).
2. PDUR transmits the PDU to SecOC to convert this PDU into a secure PDU.
3. SecOC together with CSM, verify the authenticity and integrity of the PDU.
4. SecOC returns the PDU without secure control data to PDUR to its further process if
this PDU passes the security verification. When the PDU is not “secure”, then SecOC
discard the PDU directly and PDUR does not receive any return value.
5. SecOC communicates the results of the PDU to the application too.
6. Additionally, SecOC can manage the message freshness.

![07](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b112ff17-a347-4074-b23e-f86d12b9dd96)

The secured data transmission among ECUs is based on the authenticity of the message that is composed of data+freshness+MAC value. The MAC part of the authenticated message is generated internally by the sender and it is based on the private key shared among ECUs. 

Autosar does not specify a freshness definitive calculation. Autosar modules only provide Freshness value callout from a module called Freshness Value Manager (FVM). Freshness is the mechanism to avoid malicious replication of messages that are exchanged between ECUs. These malicious replications are progressive changes without detection. To avoid these replications, some Freshness mechanisms are defined:
1. **Message Counter Based Freshness** (MCBF). Each time a message is transmitted, one
internal counter which is independent from the ECU will be incremented. The
disadvantage of MCBF is that counters shall be persistente and will add CPU load due
NVM processing. Another disadvantage of MCBF is its relative weakness from out of
sync counter conditions.
2. **Trip Counter Based Freshness** (TCBF). TCBF is incremented when a trip is
completed (ECU message process). FVM increments the TCBF along with a synch
message (TripResetSynchMessage) to both, the sender and receiver. TCBF is based on
the hybrid sending of freshness based on 3 counters to support automatic resync:
  1. Trip Counter. ECU incremental persistent 4 bytes for each ECU process.
  2. Reset Counter. PDU incremental no-persistent value, this counter increases itself when a message counter has suffered an overflow.
  3. Message Counter. PDU incremental no- persistent value, this counter increases itself every time a message is sent.
3. Time Stamps. Verification for each ECU with a real time value, time stamps shall be sent by “secure” messages.
4. Hybrid mechanisms. Combination of Timestamps and Freshness counters.

Out of sync counter conditions are conditions where counters from 2 ECUs mismatch when a communication between these 2 ECUs is in progress. This mismatch causes freshness incongruence and to solve this incongruence there are ways to solve such as:
1. Truncated freshness. This approach recognizes small differences in the freshness values from two ECUs. For example, sending values with a resolution of 10, when it is dictated that the other ECU has freshness between 11 to 19.
2. Challenge-response protocol. This approach detects large differences in the freshness values from two ECUs. Challenge-response protocol a “challenge” special message from one ECU to another to perform a sync execution.
3. Periodic counter-message. This approach injects periodic messages to the schedule to verify the counter from both ECUs and thereby avoiding out of sync conditions.

# Energy Management
Energy Management permits the energy management of various ECUs depending on the vehicle mode that is active (Vehicle mode, Application Mode, or BSW mode). The energy management gets support from the Virtual Function Cluster (VFC) that permits grouping the communication to SWCs port level based on the functionality that these SWCs perform into the vehicle, in this way permitting the activation and deactivation of this cluster when this functionality is not required. For example, CAN support a feature called Pretended Networking that can turn off the communication of CAN of one ECU while the bus is still active.
