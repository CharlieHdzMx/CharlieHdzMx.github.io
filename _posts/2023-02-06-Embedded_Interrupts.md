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
1. Interrupt. Asynchronous signal that external devices trigger over the processor.
2. Exceptions. Logical or conditional software error that can be also used for debugging purposes.
3. Trap. Synchronous internal interruption made by the processor.

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
