---
layout: post
title: Autosar Multicore
excerpt: Autosar Multicore
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Multicore
comments: true
category: blog
---
Multicore Fundamentals
Multicore is the concept that indicates that a microcontroller contains mechanisms to run activities in parallel based on the number of cores that it contain. The principal reasons to use multicores microcontrollers instead of a single-core microcontroller are:
Do more in less time. If there is the need to perform an appropriate task in the less possible time, a multicore microcontroller can split the task into different cores to be able to fasten the procedure.
Distribute functions among cores. When a set of functions are independent between each other and have a joint at the end of the process, it is possible to distribute these functions between cores. <MC1 image>
Task priority separation. If it is required to make specific tasks to not be interrupted. It is possible to separate the processing of high priority or low priority among cores. <MC2 image>
Multicore Terminologies
The execution time is the time that takes a specific amount of cores T(n) to complete a workload. This execution time can be compared with the execution time of a single core T(1)  to determine how much this workload can be reduced. Nevertheless, consider that T(n) depends on the workload structure and that the logic can inhibit the time reduction from multicore. 
The workload structure can be divided into sequential workloads and parallel workloads. The sequential workload (Ws) is the set of tasks that have inherent order constraints and cannot be processed parallelly by the multicore. Examples of the sequential workload are the initialization, shutdown, and I/O sequences. The parallel workload (Wp) is the set of tasks that do not have order constraints and can be executed parallelly.
SpeedUp of N cores (Sn) is the comparison of T(n) against T(1). Using Sn, it is possible to see the proportional reduction of time to the Wp from a workload. The core increment would not reduce the workload proportionally, due that the workload structure can be feasible to a limited number of cores, and adding more cores would not reduce the execution time.
<MC3 image>
The overhead of cores O(n)  is the reduction of Sp in the function of the increment of the number of cores to a specific procedure. At a specific threshold of the number of cores, the Sp gets to the maximum value and if more cores are added, O(n) would increase since these cores might not provide significant process improvement and, instead, they would add more management processing (core switching or monitoring instruments management) that would increase the system processing load.
Based on Amdahl’s law, the Sn is fixed to make the execution time to be the minimum possible, then a workload re-design is possible to perform to add new functionalities to make Wp greater (minimizing the Ws part).
Parallel Environment
Shared resources are common resources that are used between cores. Resources can be I/O memory partitions, device parameters, and so on. The following questions help to consider the adequate use of shared resources:
What type of shared resource is?
What is the behavior of this type of shared resource on concurrency?
What kind of connection do the cores need to manipulate this shared resource?
How long can the shared resource be connected?
Is there a mechanism to support the synch access?
What is the risk of errors and how to mitigate them?
To avoid inconsistency and concurrency misbehavior, such as deadlocks, the shared resources shall be handled with sync or lockdown mechanisms.
Exclusive Area avoids the shared resource incongruence that is accessed by different tasks by the blocking of access to all the tasks but only attending one task at that time. 
In the following image, the Exclusive Area mechanism is shown: Task2 is the first task to access X shared resource, this action blocks the access to X for all other tasks that must wait until Task2 stop using X to be able to access this resource. When it is the turn of Task1 to access the modified X value, Task1 will keep the X resource to itself in the exclusive area until it stops using X; all other Task would wait until Task1 finish. 
<MC4 image>
The recommendation is to avoid the use of shared data as much as possible because even when the shared resource incongruence is being solved, the execution of the task is being serialized and the Sp ratio would be reduced.
There are specific SW entities that cannot be parallel-accessed, one of these SW entities are the memory peripherals or external hardware devices. In these cases, Sp would not be improved due that cores shall be accessed sequentially. To improve the memory access performance, Caches or RAM mirrors are created in each core to permit faster access to the memory value.
Autosar Multicore Concepts
The Autosar multicore paradigm is based on the static mapping of cores during the configuration phase. Runnable and tasks are activated based on the core execution limitations during run time. This means that Autosar multicore support is more about SW mapping than multi-thread programming.
To support multicore, SW Components can be divided into partitions and be assigned to different cores, this means that all the SWCs with a common ASIL level can be group together to be executed on a specific core, thereby an association to priority strategies is possible. For example, while a core is executing all the ASIL-D SWCs runnable (high priority), the other cores can execute the remaining QM SWCs runnable. Moreover, every core can have different partitions and each partition might have memory protections and mapping.
<MC5 image>
The Autosar concept of the core can behave as:
Master Core. The first core that initializes on startup and can initialize slave cores.
Slave Core. Cores that depend on the master core commands.
BSW Core. Master or Slave core specialized to store the BSW execution.
Regarding multi-core support, Autosar has these general requirements:
There shall be at least one partition defined for every core.
There shall be at least one BSW partition defined for every core.
RTE shall be shared by all the cores.
Virtual SWC is shared between cores.
The BSW modules:
Can be stored entirely in a core.
Can be duplicated among cores.
Can share services by using proxies between cores.
Can assign a master partition to delegate modules between cores.
The core mapping implies implicitly that Autosar OS is only executed on one core. Each task and ISR shall be mapped exactly with their corresponding OS application and each runnable shall be mapped to its task in the same core. Thereby, every OS application is defining a partition and it is referenced in a specific core.
<MC 6 image>
Autosar describes 3 types of proxies:
SWC proxies allow the functionality distribution to application level to improve the workload, for example, connecting an SWC from core 0 to an SWC from core 1.
RTE proxies allow the partition and core communication through RTE, for example, the BSW communication between SWC from core 0 and core 1.
BSW satellites allow the functionality distribution of BSW across cores, for example, having a litter version of BSW on different cores.
RTE feature from Multicore
The interfaces from the sender/receiver and client/service communication support the multicore features. Both interfaces use APIs from the Inter-OS Application Communication (IOC) that permits the communication between OS applications that are located in different cores. This communication is performed using a protected area for global shared memory and uses callbacks to notify the data reception.
Sender/Receiver interface is a communication n:1 or 1:n that is based on data elements. These data elements are sent by the sender and are requested by the receiver through runnable. The calls from Sender/Receiver are synchronized and shall be in the same context as the sender. The communication limits are ports that can be configurable based on the runnable of the specific SWC. The communication is done by the RTE basic functions RTE_Write and RTE_Read.
<MC 7 image>
RTE_Write and RTE_Read are the simplest interfaces of enqueue data transference. When an SWC sends data through RTE_Write, RTE writes to the OS application with the function Ioc_Write(), This call permits that an SWC from another core can access this data. There are two additional APIs, RTE_receive and RTE_send that are similar to RTE_write and RTE_read but with a queued transference of different data types with time stamps. When SWCs require multicore transference of data, it is done through RTE proxies to communicate with the specific COM core. 
The Client/Server interface is a communication n:1, the call to the server is implemented from runnable of type server-SWC and can be synchronous or asynchronous. These runnable can also use the client context (unmapped server) or inside the runnable-own-context (mapped server).
<MP8 image>
When a client SWC1 invokes a server function from SWC2, it does by using the Rte_Call function. This call triggers the storing of the function argument on a temporal buffer and turns on an OS event to activate the RTE proxy from the SWC2 core; this action blocks the task until everything is done. Parallelly, the server is receiving the event from the SWC2 that later returns the value inside the temporal buffer. In the context of multicore, Client/Server communication is Sender/Receiver communication but with a translate chain process, so in this case, the Sender/Receiver communication might bring better performance than Client/Server.
<MP9 image>
Autosar Multicore OS
The multicore OS features are extensions from the basic Autosar OS features, this extension contains the following considerations:
Each core shall have an independent schedule.
Times between core responses cannot be deterministic since there is no certainty that the cores will run exactly at the same time without delays.
Not all the cores require to support Autosar OS.
Every core that supports Autosar OS would have their independent schedules being run by an OS application that allow them to run parallelly. Synchronization mechanisms are mainly required when an OS application requires data from another OS application that is being run in another core. Some synchronization mechanisms are the following ones:
Interrupt Lock blocks the execution of interruptions while non-atomic schedule tasks are being performed. It is mainly used to block time stamps. <MP 10 image>
OS-Resource blocking is used when a task requests the resource manager to block a specific resource until one specific API indicates that the resource can be freed. <MP 11 image>
OS-Spinlocks allows the blocking of a resource called Spinlock among cores. A spinlock would keep spinning a resource until it is freed, during all the time that the resource is spinning, it would be recognized as a critical section to access. Moreover, spinlocks are optimized by Autosar to support situations where they are accessed by tasks of the same or greater priority (or ASIL level).   <MP 13 image>
Deadlock is the blocking of a taskA that depends on another taskB to terminate its process, but taskB depends on taskA so a closed cycle of dependencies is present. Some Autosar Multicore support contains integrated mechanisms to prevent deadlocks, these mechanisms are usually omitting Complex Device Drivers (CDD) implementations, so CDD deadlocks mechanisms shall be developed manually by the developers. The following points consider CDD deadlocks development guidelines:
Minimize the use of shared resources.
Obtain a resource until the last one has been released (chain resource usages).
Request only for resources that had been freed.
Sorted resources access (for example, last in – last out)
Priority ceiling that sets directly a higher priority to a task to take over the resource usage from another task.
<MP14 image>
Autosar Multicore Integration
In startup time, Autosar initializes a master core, this master core, later on, initialize the slave cores; slave cores can initialize other slave cores as well. The synchronization of cores during startup happens twice, one time before executing the Startup Hooks and another one before starting the habitual scheduling.
<img 1>
The startup code requires to be called before the main() function, which is the function that calls the principal startup function of Autosar (Os_Init() and Ecum_init()). Some principal activities of the startup code are:
Microcontroller configuration.
Memory and registers initialization.
Base address initialization (interrupts and vector table).
All the cores shall execute their dedicated startup code, Autosar requires the master core to initialize itself at the power-on time; while the other cores can be initialized using the OS initialization.
During the pre-OS phase (before the synchronization of OS init-core), the function EcuMIniti() is called, which initializes the ECU manager to check if the master core is present to command the unique initialization of ECU drivers, this initialization cannot be done by any slave core. Later on, all the slave cores are initialized, this initialization is performed by the increment of counters of the master core through the API StartCore() until this increment match the number of available cores. After EcuMInit() had returned and all the slave cores had been initialized, EcuM_StartOS() is called to start the pos-OS phase.
The first synchronization during the Startup phase waits that all the cores had been initialized, the second synchronization is the Startup phase that makes that the master core waits until all the slave core RTE context had been initialized correctly. Before initializing their own RTE, the slave cores check if the master core RTE is already started.
The shutdown process is started by the Mode Management of BSW and EcuM and using a sequence of state changes such as the following example:
BSW Core state == RUN
Publish shutdown request
Apply housekeeping throughout the BSW modules.
BSW Core state change to PrepShutDown
ShutdownFinished is set.
EcuM_GoDown() called
The Ecu is off.
The shutdown is triggered by the ShutdownAllCores() function from the master core (slave core cannot command the ECU shutdown directly), then the master core distributes the shutdown request to all the slave cores, and all the cores would call EcuM_GoDown() to request the Shutdown Hook from their OS application, at this point, one synchronization among cores is performed before calling all the Shutdown hooks. In the end, one callout of the master called EcuM_WaitForSlaveCores() is waiting for the slave core shutdown to be ready to turn off the ECU entirely.
Multicore BSW Split Feature
BSW modules can consume a high percentage of the available workload and timing. To avoid this, one standardized approach is to split the BSW among cores to distribute the load of processing. This BSW split is possible because Autosar design allows some BSW modules to be executed between cores such as DEM, EcuM, or PDUR.
BSW split can encounter different challenges:
Context change. When a BSW module “A” is moved to another core, any API from any lower layer or upper layer which directly depends on module "A" would generate a higher overhead because instructions to save the context will be added for each single communication between cores.
Data Lifetime. When a BSW module “A” is moved to another core, any API from the lower layer or upper layer that its module is not present in the same core of “A” would require an additional buffer located in the proxy to save their data during the context change. If this is not done, any data that is referenced in the context will be deleted when there is a context change.
Synchronized communication between context changes. Synchronization is required when output values are required immediately during context changes. This requirement requires additional implementation such as invocations without context change or synchronized context change (default context changes are asynchronous). One implementation of synchronized context change is to trigger a value request and do not wait for a response to continue the processing; this strategy is called “Fire and Forget”.
The development of BSW split strategies requires a complete understanding of the models that are considered to be split, their sequences, their interactions with other modules, and their proxies requirements to enable the context change. To have a more efficient reallocation of BSW modules, best CPU load distribution, and interruption routine responsiveness, some Autosar dedicated vendors offer a predefined set of modules and additional functionalities to support the reallocation of BSW modules among cores.
To avoid the unnecessarily large amount of overhead and CPU load, it is recommended that the reallocation of modules among cores are done by complete clusters (for example, relocate all the modules related to the LIN communication in the service layer) to enable better and less call of interfaces. It is also recommended to relocate the set of modules that serves as the glue between other modules. For example, the following image shows PDUR is the “glue” due to its amount of interfaces related to PDU processing and its generic use of communication transport protocols; this approach does not affect the tightly coupled modules since the interfaces are not translated partially and the interfaces that are not related with PDUR are not impacted.
<Img 3>
BSW Proxies and Satellite Modules
Based on the Master/Satellite architecture concept, the satellite modules helps the efficiency of run-time processes. The Master/Satellite architecture is based on the approach of separating one complete functionality into different cores; for example, one core contains the principal logic of one module, whereas other cores contain the resources and interface logic with RTE and other modules.
<Img 4>
The proxies of SWCs are modules that translate any communication of type Client/Service into Sender/Receiver to have faster communication between cores.
<Img 5>
Multicore Design
General steps to design Autosar Multicore SW:
Analysis
Workload characterization. Apply Amdahl and Gustafson laws and use patterns to enable different levels of parallelism.
ECU and logic states. Analyze the requirements of startup, shutdown, normal operation (specially BSW state machine), bus frequency, and stress phases of I/O interfaces.
Potential of defining functionality clusters. Identify clusters with many interfaces and design synchronization among SW modules.
Design
Minimize the data exchanging and control flow between cores.
Use low profile estimations to predicate the behavior of the system (OS technical reference and run time measurements).
Development
Beware that the OS maintaining several cores can be more complex than an OS that only maintains one core. One approach to reducing the complexity of multi-core OS is by reducing the service calls from the OS through S/R communication or asynchronous C/S(“Fire and Forget”).
Use asynchronous event triggering instead of synchronous access to receive data.
Beware of preempty effects of tasks that can affect other cores.
Identify the OS with long waiting periods to access HW.
Low priority and non-preemptive tasks can reduce the synchronization time between cores because higher priority tasks shall wait for this kind of task to finish, the exception of this rule is any runnable that is sensible to jitters. Low priority and preemptive tasks require the use of additional processes to reduce their synchronization time ( such as suspending interruptions while a critical code is being executed).
Application SW components are not meant to support multicore directly, since their interface of multicore is RTE. Wherea the set of Complex Device Drivers that require any implementation of multi-core capabilities such as:
Serialized access to shared resources. This permits the shared resources access to one service at a time. This access is between SWC service calls from different cores or partitions, instances of BSW function, or interruptions.
Avoid shared resources.
Partition and branching monitoring depending on the core.
<img 6>
RMW operations Protection
Protection to Read-Modify-Write (RMW) operations are not intended to apply to Read-only files due that Read-only variables do not require any protection. One way to avoid the expensive multi-core RMW operations is to design every BSW instance to have their own RMW variable and avoid the shared resources access among cores; in this way, protection mechanism, such as spinlocks, are avoided with the tradeoff that more RAM is consumed and more development time is required to configure every core additional data.
<img 7>
Context self-awareness
The concept of context self-awareness is the way that CDDs know in which context they are located based on the APIs GetCoreID() and GetApplicationID(), these APIs perform specific branching to take data from different BSW runnable (core dependent branching). The advantage of core dependent branching is that any BSW runnable can exploit the benefits of multicore with the cost of having a more complex logic implementation and configuration. Additional logic to define proxies and satellites is required if any CDD or I/O-Hw Abstraction requires access HW directly to use them in different contexts. 
Multicore development recommendations
The data exchange at the boundaries of cores can be handled by the use of proxies and satellites. The main difference between satellites and proxies is that satellites permit better distribution of workload between cores and proxies to translate the C/S communication to a more efficient S/R communication.
Regarding SWC logic at any core boundary, is recommended to use enqueued communication because the queued communication uses spin-locks. In this context, it is also recommended to access ports with buffers instead of ports without buffers to avoid frequent access inside runnables and make data stabler during execution.
Regarding data type in atomic access, it is recommended to use simple data types instead of complex data types because simple data types have a better consistency and avoid the need for additional security measurements when they are used within a signal group. If complex data must be used, then it is recommended to combine them inside big structures to minimize the use of spinlocks.
Regarding SWC and BSW mapping, it is recommended to pass SWCs with higher internal algorithm processing to a core without BSW; otherwise, when an SWC has frequent interaction with BSW is better to let that SWC logic within the same core than BSW. The SWCs that depend on each other sequentially are recommended to be mapped in the same core, nevertheless, it is possible to separate the core of an Input-Process-Output sequence model where SWCs have dedicated functionality as Input handler, Processor, or Output handler. 
<img 8>
Regarding runnables, it is recommended to group Runnables inside an atomic SWC that is located only in one core, this is because runnables cannot be mapped in different cores. This is also preferable in Input-Process-Output model patterns because this minimizes the synchronization time.
Parallelism Design Patterns
The following points are some examples of implementation of parallelism:
Pipes. They use received data triggers among cores to perform pipelining to reduce the spinning timing. Among cores, there might be feedbacks to the previous processings and parallelism execution. <img9>
Global times. Every SWC holds a timestamp that can be started by external triggers (ET) or internal triggers (IT). The possible worst-case time from the trigger execution to the end of the timestamp is used to calculate the worst-case execution time(WCET). <img10>
Fork and Join. This implementation is based on synchronous or asynchronous calls of Operation Invoked Trigger (OIT). In synchronous calls, the execution is frost when other cores are being executed to balance the workload. In asynchronous calls, parallel activities are possible to reduce the spinning time. <img12>
Single core to Multicore migration
There are two possible cases to migrate to multicore:
Single-core to Multicore
Few cores to Multicore.
Migrate from a single core
When migrating from one core to multicore, the architecture shall be enclosed to be pure multicore and RTE would handle the multicore abstraction and task mapping to be associated with the different new cores. Moreover, all shared resources shall be extended to cover multicore concurrency.
Some risk when migrating from a single core can be:
Data inconsistency. The consistency of data that worked on single-core systems might not work in multicore systems. These inconsistencies can be hard to detect, for example, interruptions of locks, task priorities, non-preemptive tasks might differ after the migrations.
Among cores sequential calls deficit. If single core SWC shared a sequential dependency of calls, when they are moved to a multi-core system, the sequential execution might fail because every core would contain their own independent scheduling from other cores and then the sequential calls might increase the overhead and waiting time between core context changes.
Every related task and SWC runnable frame shall be mapped to the same core when the SWCs are partitioned to join different cores due that runnables are core-dependent.
Migrate from few single cores
Usually, the multicore architecture bandwidth is higher than the multiprocessor's interconnected architectures. The deletion of these interconnections allows better performance of the system due that vehicle network protocols (SPI, UART, CAN, LIN, Ethernet) require more computation to transmit data in multiprocessors system than multicore systems.
Some risk when migrating from a few core can be:
Resources reduction. For example, the memory would be shared. Even when multicore memories are designed to be higher to support multicore architectures, usually these memories can be less than the processors that were migrated.
Performance can be affected by shared resources arbitration.
Some resources drivers are not designed to be used on concurrency.
The periodic force of core power switching. In ECU interconnections, sometimes an ECU can be turned off (Low power mode), reboot, or turn on independently from other ECUs, but in multicore, there are few drivers that can support periodic power switching of cores.

