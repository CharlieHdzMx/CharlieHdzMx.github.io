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

The **Technical Safety Concept** takes the Functional Safety Requirements, Safety Goals and ASILs level to determine how to implement the Safety Requirements to a system. Technical Safety concept indicates that all HW Safety Requirements and SW Safety Requirements shall be defined along with their respective Safety tests.

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
QM SWCs shall change the OS context when they require interaction with Safe BSW; ASIL SWCs shall change the OS context when they require interaction with QM BSW.

Since OS Context changes consume CPU load and memory, the distribution of SWCs and interaction/dependencies with other ASIL level SWCs shall be well defined to minimize the frequency of OS Context changes. To decide which BSW to use, either Safe or not, depends on the ratio of ASIL SWCs/QM SWCs. However, due to trusted functions, the context change, overhead and transition time is faster on Safe BSW.

Safe BSW contains SafeNvM. SafeNvM provides safety features such as detection of corrupted or lost NVM blocks. Nonetheless, SafeNvM cannot ensure that all data are saved correctly in special cases such as having a reset before the stack is notified to write the NVM block.

SafeBSW contains SafeEcuM. SafeEcuM corrects the post-build configuration distribution, corrects initialization issues and performs resets if necessary.

## SafeOS
SafeOS provides safety against memory and timing failures. SafeOs supports FFI strategies for single and multiple cores.

Memory failures can be invalid pointers, stack overflow, writing invalid locations.

Timing failures can be infinite cycles, frequent interruptions, long executions that affect scheduling time.

SafeOs solve memory and timing failures by:
1. Memory partitions that depend on the PS application and MPU (Memory Protection Unit) protection.
2. Stack protection, sentinels and indicators that protect the stack from overflows or underflows.
3. Timing protection, time monitoring and application termination.

SafeOs contains mechanism to solve memory/timing failures such as:
1. **MPU** SelfTest, OS context changes produce reset to MPU, therefore it is needed for MPU self test during initialization.
2. **Memory Protection**
3. **Timing protection**
4. **Schedule table**.
5. **Safe context switching**
6. **Application termination**.
7. **Reset of Individual Partitions**
8. **Inter-OS application communication**

## Timing Protection
Timing protection uses execution budgets to limit the time that a task can last without affecting the schedule. For example, in the following figure, Task 1 has been delayed due Task2 and Task 3 calls. This delay causes a Deadline Violation which will call a protection hook. Protection Hook is a function that determines actions when a delay is present. The Deadline violation monitoring, then, is performed by the Application OS only for elements that have a locking deadline or execution budget.
![05](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9260b534-3cac-49ae-82c0-3c60ee7a3728)

Deadline violations call protection hooks, protection hooks can have different actions for a timing issue; rather than only resetting the system, as the Watchdog protection does.

## Partition
In SafeOs, there is an instance called EcucPartition. EcucPartition allows OS partitions to determine the object collection within an OS (Task, Interrupts, Hooks, Event, ect). These objects can interact between each other and they can access exclusive regions within the uC resources (CPU cores and memory).

![06](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b91e6954-e70e-4c0b-965a-44b6bee49a4b)

**OS Application** can be trustful or not. Trusted OS application has free access to uC resources and it can act without latent monitoring; Non-trusted OS application has limited room for action based on the configuration.

OS Partition split the memory access based on the ASIL level of the SW element. These memory partitions protect themselves by the MPU. Therefore OS partitions refer to only one ASIL level, where there is no OS partition with objects of different ASIL level.

Every context is associated with specific resources (stack, core registers, CPU mode and MPU settings). Context changes occur when switch task, ISR entry, hook routine and service calls are present from different OS applications. The context change implies that only one context can be active on an ECU.

## MPU
**MPU** is a mechanism that labels memory access based on access configurations. Overall, MPU permits distinctions between privileges to access memory regions. The configurations Start Address and End address permit if an access request contains the sufficient privileges to write, read or execute on a memory region. Number of memory regions depends directly from the microcontroller.

MPU implementations contain an API called **Memory Access Check** (MAC). MACs determine the memory access type, Readable, Writable, StackSpace) from a delimited memory area.

Same concept of MPU is used on the Peripheral Protection Unit (PPU). PPU can be defined for special peripherals, while others can use MPU instead.

## Stack Protection
Stack protection can be done by HW or SW. HW Stack protection is based upon MPU and the OS application property of accessing the stack (either trusted access or not). The SW stack protection is based upon the stack delimiters patterns checking; which can detect stack overflow or stack underflow. If SW detects a change of the pattern value, which is located in the limits of the configured stack size, then the Stack protection will call a Shutdown Hook that can be implemented by the user. This SW protection can be configured by OsStackMonitoring configuration.

The MPU Stack protection indicates a privilege access region for each task. This access is delimited by defined symbols, if the MPU needs reflashing (due a recompilation), these symbols will change dynamically.

The difference between the HW protection stack and the SW protection stack is that HW MPU does not permit writing inside the privilege stack area whereas SW stack protection allows the writing of stack areas and later support their requests to have an ECU reset.

## Trusted Function
A trusted function (**TF**) is the method which has a Trusted attribute that gives special access to resources. This attribute can be assigned by either a Trusted OS application or a non-Trusted OS application. TF is the flexibility to permit unlimited access to privilege resources. For example, a TF can share the general stack from the OS application.

A non trusted function (**NTF**) is the method from an untrusted OS application that has limited privileges. A NTF call causes the following actions:

1. OS context change, if before the NTF call, the system was functioning with Trusted OS application context.
2. Stack assignment, exclusive to prevent access to trusted OS application info while preempting.

Instead of returning a value and saving in it to the TF stack, A NTF writes this value at a global pointer that can be accessed by the TF stack and the NTF stack as well.

## Memory Mapping
The memory mapping is involved at the beginning of the compilation process. Memory mapping is located at .map file, which is generated together with the executable binary file .efl. Both files are generated after the linker and location process. .Map files show symbols grouped based on their type, code, const or variables, and the OS application that uses them. When grouping these symbols, it is possible to delimit TF symbols from NTF symbols; therefore memory mapping supports FFI due it separates elements with different ASIL levels. Nonetheless, there are also global symbols that can be accessed by all OS applications.

Memory mapping configurations are used to define the memory address for the symbols, the access type (read, write, R/W or X), if it is accessible for the supervisor or user, and so on.

## SafeWdg
Failures while executing code can vary as follows:
1. Code is being executed before or after expected.
2. Code being executed without request.
3. Too long code execution
4. The code flow mismatch against the expected flow.

Some ways to inhibit these failures:
1. **Deadline monitoring**, aperiodic execution application which compares the time between 2 delimited checkpoints (max and min).
2. **Alive monitoring**, periodic execution application which monitors “checkpoints” periodically.
3. **Logic monitoring**, incorrect execution application which validates checkpoints sequence against predefined sequence.

The watchdog (**WDG**) can detect SWC and BSW timing failures. When a timing failure is present on a SW supervised entity (SE), the Wdg will trigger an ECU reset. Incongruencies of checkpoints can be inherited by different ASIL level SWCs, so non OS application context change is required.

Wdg Manager (**WDGM**) monitors SEs, when a SE has any timing failure, WDGM returns a value of NOK, this value causes a set of steps to ECU reset. 

When _Wdg_Init_() is called after _StartOS_(). This is because _WdgM_Init_() depend on the _StartOS_() critical session exiting. _WdgM_Init_() will call the main function of WdgM periodically after initialization.

The different configuration of a ECU Wdg are:
1. **Internal Watchdog**, embedded into the uC.
2. **External Watchdog**, outside the uC.
3. **SBC**, system basic chip outside the uC.

These configurations use different SW modules to work, but in all cases the WDGM and WDGIF are used. SBC and external watchdog can use Complex Drivers and bus communication modules to communicate with the uC.

The WDGM provides monitoring and control methods from WDGIF to other modules. WDGM can trigger immediate resets (primary path reset) or uC driver processing resets (second path reset), the MCU driver can incorporate strategy before doing a reset when second path resets are requested; for example, debouncing the signal to prevent false alarms.

WDGM will supervise a SE during SE lifespan based on a configured WdgMSupervisionCycle period cycle. This supervision resets the HW counter of the WDG. If the SE overpasses the execution time, then WDGM will trigger an ECU reset.

## Supervised Entities, Checkpoints y Transitions.
A SE is the SW abstract entity that is supervised by WDGM to prevent timing failures. A SW contains 4 states that permit WDGM to acknowledge if SE has a failure or not. The SE principle states are Ok, Deactive, Expired and Failed. The difference between expired and Failed is that when a SE failed, failures can be recovered while when a SE Expired, any failure is not-recoverable.

A **Checkpoint** (CP) is a control point within the logic flow of a SE. CPs notify WDGM when the SE logic reaches a key point. CPs can be called directly by CheckpointReached() or indirectly by RTE. CPs are exclusive from SEs and each SE must have at least one Global Initial CP and optionally global and local intermediate CPs. CP algorithms can determine if the SE logic flow is correct based on pre-configured comparison. These algorithms can monitor the execution time between CPs or perform an alive counter.

**Transitions** are flow references between checkpoints. Transitions can be global (checkpoints between SEs) or local (checkpoints within SEs).

## Alive Monitoring
Alive monitoring is a periodic monitoring of a SE by counting how many times a SE Checkpoint is reached. For live monitoring, it is only necessary one CP and the application shall be synchronized along with WDGM during initialization and within the main function. Main function evaluates the specific period based on the Supervision Cycle configuration.

![07](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/32626729-836c-4069-ac1e-f44626288870)

The **expiration tolerance** configuration is the Wdg configuration that permits a reset is delayed after WDgMSupervisionCycle when a CP is not reached. Even when the CP is reached after this delay time, this tolerance will not permit the Wdg recovery and will perform a reset as soon as the delay is expired. On the other hand, the failure tolerance configuration has the same logic as the expiration tolerance but allows the recovery when a CP is reached after its delay time.

## Program Flow Monitoring

The **program flow monitoring** checks if a SW SE entity sequence is being executed using the expected configured sequence. This flow is monitored by CP transitions, and therefore it is required at least one CP. When a difference between the performed sequence and the preconfigured sequence is discovered, the calls of Checkpoint_Reached() will differ and the SW will detect the error; but actions will be taken until Wdg MainFunction() is called and detects an “expired” state. Wdg MainFunction() will follow the configuration to handle the “expired” state (primary or secondary path, failure tolerance, expiration tolerance, ect).

![08](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/9cff8b45-3265-4e3e-a633-80ae766a083e)

Transitions between SEs have rules, if any of these rules are broken, then the Program Flow Monitoring logic will set a reset. Some of these rules are:
1. Global End CP that connects 2 SEs shall not have more than one input transition.
2. Global Initial CP that connects 2 SEs shall not have more than one output transition.
3. Global transitions between SEs shall be by a middle CP.
    
![09](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/0c1ae6ea-6200-485c-8ef9-c6b79a809b4e)

## Deadlines Monitoring
**Deadlines Monitoring** uses the same logic than Program Flow Monitoring but adding the minimum time and maximum time of execution. Minimum time and maximum time are references to notify deadlines (crashed tasks or infinite cycles). The way that Deadlines works is by the **timestamps** between CPs. Each transition has a specific time stamp, if a transition is not on the correct range, then when the Wdg _MainFunction_() is called at the end of the Supervision cycle period, Wdg will report the failure.  

The time stamp is managed by WdgM and can be externally or internally configured. If the timer is internal, then it is based on the ticks of WdgM; if the timer is external, then WdgM will ask for the increase of the counter by calling _WdgM_UpdateTickCount_(). Internal timers are used for transitions that are higher than the Supervision Cycle, whereas the external timers are used for transitions that are lower than Supervision Cycle.

Specifying all CP output transition deadlines is recommended to avoid masking a failure from the CPs outputs. For example, if CP0 has output transitions to CP1 and CP2, and deadline time is defined for CP2 but not for CP1; then CP1 can mask a failure from CP2.

## Fault Reactions
The principal reaction to a failure is the transition of the system to a Safe state. One method to reach the Safe state after a failure is by resetting the uC. This can be done principally by the Wdg. Additionally, the failure can be reported by DEM BSW module (Diagnostic Event Manager) and/or performing a OS application context change based on configurations.

The **Fault Tolerance Time Interval** (FTTI) is the failure duration until the system can have damage. FTTI is the sum of the Failure detection time (**FDT**), the Failure Reaction Time (**FRT**), and the time that Safe state mechanisms need to start to avoid the damage. In the majority of cases, the Safety Requirements requires FTTI to be as low as possible and this request depends on the ASIL level.

![10](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/293dd805-6796-4e5a-9b77-f6c5fe211060)

## SafeE2E
Safe E2E protection solves failures during communication between ECUs. Some E2E failures are:
1. Lost messages
2. Delayed messages
3. Incorrect insertion of messages.
4. Undesired duplication of message
5. Message corruption

The principal ways that Safe E2E solves these issues are based on:
1. CRC: Numeric value based on the message content. CRC detects message corruption.
2. Sequence counter: Undesired duplication message counter.
3. Receptor side timer to determine if a message got lost.

These solutions can be grouped together into two main safe mechanisms: Protection Wrapper Solution and Transformer Solution.

### E2E Protection Wrapper

E2E Protection Wrapper (**E2E PW**) overwrites the traditional RTE function Rte_write and Rte_read for its own E2E PW wrapper (E2EPW_Write and E2EPW_Read). To do this, E2E PW needs the E2E and CRC libraries, and it only works for conventional I-PDUs.

E2E PW is an intermediate entity between SWCs and RTE. When E2E transmission is required, PW attachs the counter and CRC to the message and forward this message to RTE. Otherwise, when E2E reception is required, the E2E PW takes the message and processes the CRC and counter before passing the message to the SWC. Aditionally, while intermediating messages between SWC and RTE, E2E PW can report the status of the transported messages.

![11](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/f47884fa-aaaf-40a7-b01c-82fadeed9fa3)

### Transformer Solutions

Autosar specs dictate that E2E cannot be processed inside BSW, due BSW modules alone cannot be ASIL compliance and E2E protection brings private data from SWCs. The RTE commands communication service clusters to permit the E2E communication. These services process the information from RTE transformers. RTE transformers are responsible to serialize(complex data to byte arrays) and deserialize(byte arrays to complex data) data from/to SWCs.

RTE transformers buffers can be configured to be in-place or out-place. In place buffer is used as input buffer and output buffer at the same time, reducing the use of memory but increasing the CPU load. In other hand, out-place buffers are two dedicated buffers: one input buffer and another output buffer; this consumes more memory but reduces the CPU load.

**Transformer chains** are a sequential set of transformers that are grouped together. Each transformer performs a transformation over the data and pass that transformed data to the next transformer. The principal transformers are:

1. COM based transformer (**COMXF**): Group signal transformer type that its transmissions are connected to COM BSW module. These group signals are transmitted by Sender/Receiver PDU interface type with a constant length and byte interpretation.
2. Some/IP transformer (**SOME/IPXF**): Can be transmitted by S/R and C/S interfaces, this transformer performs serializations and deserializations to communicate with LDCOM (Large Data COM).
3. E2E Transformer (**E2EXF**): use Client/Server communication, both queued or unqueued. It is only used between cyclic communication. E2EXF replaces E2EPW APIs.
4. 
![12](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b8106eee-48c1-498d-96d2-a351bc23f403)

![13](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/16cbd571-721d-445b-9ca6-8fef8dc8a05f)

## SafeRTE

RTE is the module that communicates SWCs with BSW. **Safe RTE** is an extended RTE that ensures the detection and prevention of communication failures between SWCs and BSW. Safe RTE has mechanisms to allow different ASIL SWCs communication, these mechanisms can produce OS application context changes to communicate ASIL SWCs with QM BSW and vice versa.

Safe RTE mechanisms can fetch data indirectly from the RTE, these mechanisms ensure FFI between two different ASIl level components. C/S proxy mechanism can be used to generate containers of output parameters and input parameters to indirectly access values, and in this way ensuring FFI.

### Tool Confidence Level

RTE is not generated statically, then the **RTE generator tool** has the responsibility to ensure Safety on RTE. The nomenclature to determine the truthfulness of RTE generator tools is the Tool Confidence Level (**TCL**).

TCL is derived from Tool Impact Level (**TI**) and Tool Error Detection level (**TD**). TI is the possibility that the tool introduces errors to the generation of RTE. TD is the truthfulness of the tool to detect errors. TCL level is calculated based on TI and TD, this level determines how much effort shall the tool/user perform to ensure RTE safe state. Using Safe RTE, the TCL level is minimized. To reach the highest level of truthfulness (TCL1), the user shall perform assessments inside and outside the RTE generation tool. Some of these assessments are:
1. Additional self check and validation.
2. Intensified Tests
3. Solution assessments and reviews
4. Extensive analysis of RTE generation scenarios.
   
![14](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/2b970f4a-e573-4e27-abf2-2bdfe537537b)
















