---
layout: post
title: Embedded Interrupts,Scheduler and Tasks
excerpt: Embedded Interrupts,Scheduler and Tasks
modified: 5/07/2021, 9:00:24
tags:
  - C
  - Embedded Systems
  - Interrupts
comments: true
category: blog
---
# Interrupts
Interrupts are essential features designed to prompt the processor to respond to various events. One primary use of interrupts is to prioritize real-time events, especially those that are time-critical, over the execution of the main program. They enable the processor to efficiently handle external or internal events as they occur, allowing for timely and responsive processing of critical tasks without compromising the overall program flow.

The different features that interrupts encapsulate are:
1. **Interrupt**. Asynchronous signal that external devices trigger over the processor.
2. **Exceptions**. Logical or conditional software error that can be also used for debugging purposes.
3. **Trap**. Synchronous internal interruption made by the processor.

If the processor's design incorporates a dedicated pin for interrupts, there needs to be a mechanism for identifying the source of the interrupt (where it originated). This is crucial for scenarios where multiple devices and peripherals share interrupts. The processor must have the capability to discern which specific device triggered the interrupt, enabling it to process the interruption accordingly. This identification mechanism is essential for efficient and precise handling of interrupt-driven events in a system.

Interruptions has “**attributes**” to determine its priority, disabling state, edge sensibility, level sensibility, and other custom “attributes” that are based in the specific processor.

## Priorities

In situations where multiple interrupts occur simultaneously, processors typically have a priority scheme among their interruptions. This priority system allows the processor to determine the order in which interruptions are dispatched. By assigning priorities to different interrupts, the processor can decide which one to address first, ensuring a structured and controlled handling of concurrent interrupt requests. This prioritization is essential for managing the timely and efficient processing of various events in a system with multiple interrupt sources.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/6b6bea29-e996-431f-af27-d4ef33029f88)

Interrupt priority can be configured by hardware (HW), software (SW), or a combination of both. In some microcontrollers, there might be dedicated pins or mechanisms in hardware to assign high priority to specific peripherals. On the software side, the processor can implement algorithms to dynamically determine and manage interrupt priorities based on the current system state, requirements, or predefined rules.

## Disabling State
Maskable interrupts are interrupts that can be disabled or "masked" based on the design of the processor. The processor typically provides the capability to selectively enable or disable specific interrupts.

Non-maskable interrupts (NMI) are high-priority interrupts that cannot be disabled or masked by the normal interrupt masking mechanisms. They are typically reserved for critical events that require immediate attention, they are often positioned just below the reset interrupt in terms of priority. It is important that if you disable interrupts, you should reenable them to avoid unexpected
behaviors.

## Edge Sensibility
Edge sensitivity in the context of interrupts determines whether the processor should trigger an interrupt when it detects a transition from LOW to HIGH (rising edge) or a transition from HIGH to LOW (falling edge). The interrupt controller is responsible for managing this transition and signaling the processor to dispatch the interrupt. Once the interrupt is triggered, it typically remains active until a new transition occurs, as indicated by the interrupt controller. This mechanism allows the processor to respond to specific changes in signal states, ensuring that interrupts are appropriately handled based on the configured edge sensitivity.

# Level Sensibility
Level sensitivity in the context of interrupts determines whether the processor should maintain the interrupt active for a certain duration, either in a HIGH state or a LOW state. Unlike edge sensitivity, which responds to transitions, level sensitivity keeps the interrupt active as long as the signal level is maintained at a specific state.

The interrupt controller is responsible for managing this duration and signals the processor to maintain the interrupt for a certain period of time. This mechanism allows the processor to respond to continuous or prolonged conditions signaled by the interrupt, providing flexibility in handling different types of events in the system.

# Scheduler
In the early days of embedded systems software development, developers had to manually control all aspects of the processor states and configurations, along with writing the application software. However, in modern embedded systems, there are operating systems that allow the execution of tasks either in parallel or in sequence. This shift permits developers to focus more on the application software.

Tasks, in this context, refer to independent pieces of software that can run concurrently or sequentially, separate from the main operating system. For example, the `main()` function in traditional embedded applications is a task that can run from the moment the embedded system powers on and continues running until it is powered off. The introduction of operating systems in embedded systems has brought about a more structured and modular approach to software development, enabling greater complexity and functionality.

To manage independently running tasks, a schedule is essential. A **schedule** acts as a routine that oversees the task processes. It facilitates the switching between groups of tasks based on the configuration of the processor or specific conditions within the current routine. This dynamic switching ensures that tasks are executed in an organized and controlled manner, allowing the system to effectively handle multiple tasks concurrently.

The scheduler employs the concept of multitasking, where various groups of tasks take turns executing themselves. The scheduler process can be configured in various ways to accommodate different system requirements. Simple configurations include a time-expiring set of tasks, where tasks are executed based on predefined time intervals. More complex and often more useful configurations involve tasks with priorities.

By utilizing priority within tasks, the system gains the ability to preempt a lower-priority task, allowing a higher-priority task to run. Preemption, in essence, involves temporarily pausing the execution of a task. This pause involves saving the task's registers to the stack, preserving the address of the program counter (PC), and so on. This process allows the higher-priority task to execute. Preemption can be nested several times, but at the conclusion of the higher-priority task's execution, the lower-priority task can recover its saved state and continue its execution. This mechanism is crucial for managing task priorities dynamically and efficiently in embedded systems.

In a scheduling system, there are typically both **background** and **foreground** **tasks**. The background task runs only when there are no foreground tasks in need of execution. This implies that the background task has the lowest priority and serves as a secondary operation that can be performed when the system is not actively engaged in more critical foreground tasks.

The background task is often utilized for operations such as saving data in nonvolatile memory or performing calculations to generate more precise data than what is currently in use. This segregation of tasks into background and foreground allows for efficient resource utilization and ensures that critical foreground tasks take precedence over less time-sensitive background operations.

## Types of Schedulers
There are different types of schedulers, each designed to meet specific system requirements:
1. **Time-Execution Schedulers:**
   - Developers predetermine the time required for each job (set of tasks).
   - Assign specific time intervals (e.g., 5ms, 10ms, 1s) for periodic execution of these tasks.
2. **Resource-Calculation Schedulers:**
   - Operating system dynamically manages jobs based on available resources.
   - Can ignore or postpone tasks when there are insufficient resources to execute them.
3. **Priority and Deadline Priority Schedulers:**
   - Tasks are assigned priorities, and additional consideration is given to deadlines.
   - The primary priority is calculated to determine the highest-priority task with the nearest deadline.
   - Involves more computational overhead but provides effective task prioritization.

Each type of scheduler offers a different approach to managing task execution, allowing developers to choose the one that best fits the requirements of their embedded system.

# Tasks
Tasks within an operating system software have states to indicate whether they are running, ready, or waiting. In real-time systems, only one task can be in the running state at any given time. Other tasks are in the ready state if they are waiting for the currently running task to finish or if they are awaiting an external event to become ready for execution.

The scheduler is the entity responsible for managing these states. It has the ability to transition tasks from the ready state to the running state based on priorities, periodicity, and deadlines defined for the set of tasks. The scheduler plays a crucial role in determining the order and timing of task execution, ensuring that tasks are executed in accordance with the system's requirements and constraints. When a task is completing its execution, the scheduler maintains a queue of ready tasks sorted by priority. The scheduler then selects the highest-priority task from this queue and performs a context switch to switch the execution to the chosen task. In real-time systems, it's crucial for this context switch to be as fast as possible to meet timing constraints. For efficiency and speed, the context switch code is typically implemented in assembly language and is closely associated with hardware specifics. This low-level implementation allows for a rapid transition between tasks, minimizing the time overhead associated with the context switch and ensuring the system's responsiveness to real-time requirements.

Setting task priorities requires careful consideration, as inappropriate prioritization can lead to various failures, especially when the CPU overhead approaches its limits. The task context, which includes attributes such as task priority, current state, and task identification, plays a crucial role in managing the execution flow.

If a system defines too many high-priority tasks or tasks that frequently block the scheduler, lower-priority tasks might face execution issues and result in system failures. To address this, solutions include upgrading to a faster processor or reevaluating the priority and design of tasks. Adopting a static scheduler where the highest priority is assigned to the shortest tasks can help optimize the system's responsiveness and prevent issues associated with task prioritization.

# Race Condition
In systems where resources are shared among threads or interruptions, race conditions can be a significant source of bugs. **Race conditions** occur when multiple threads or interruptions access shared resources concurrently, leading to unpredictable behavior and potential errors. To maintain system stability and reliability, it is crucial to avoid race conditions.

Strategies for avoiding race conditions include:
1. **Mutexes (Mutual Exclusion):** Use mutexes to synchronize access to shared resources, allowing only one thread or interruption to access the resource at a time.
2. **Semaphores:** Implement semaphores to control access to resources, ensuring that a limited number of threads or interruptions can access the shared resource simultaneously.
3. **Critical Sections:** Identify critical sections of code where shared resources are accessed, and use synchronization mechanisms to protect these sections from concurrent access.
4. **Atomic Operations:** Utilize atomic operations to perform operations on shared variables as a single, uninterruptible operation.

By employing these synchronization mechanisms and carefully managing shared resources, developers can minimize the risk of race conditions and enhance the overall robustness of the system.

Race conditions result in outputs that vary based on the order in which instructions within a function are executed, whether by the application or Interrupt Service Routines (ISRs).

An illustrative example of a race condition occurs when the main program and an Interrupt Service Routine (ISR) share the same variable, let's call it "X." In this scenario, when an interruption occurs, the ISR increments the value of X, while the main application decrements X when its value is non-zero. If the application modifies X and, just before updating the value of X into memory, an interruption takes place, the ISR may also modify the value of X. The critical issue arises when the context saved from the application "overwrites" the ISR-modified value, effectively returning to the original X as if the **interruption never occurred**. This situation highlights the potential conflicts and unexpected outcomes that can arise from race conditions in concurrent program execution.

To address race conditions, the section of the application where the variable X is modified should be enclosed within a block where interruptions are disabled. This ensures that the modification of X is treated as a critical section, where only one operation can be executed at a time. It's important to minimize the number and duration of critical sections to prevent slowing down the system response. 
