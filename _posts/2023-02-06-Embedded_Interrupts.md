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

Interrupts are essential features designed to prompt the processor to respond to various events. One primary use of interrupts is to prioritize real-time events, especially those that are time-critical, over the execution of the main program. They enable the processor to efficiently handle external or internal events as they occur, allowing for timely and responsive processing of critical tasks without compromising the overall program flow.

The different features that interrupts encapsulate are:
1. Interrupt. Asynchronous signal that external devices trigger over the processor.
2. Exceptions. Logical or conditional software error that can be also used for debugging purposes.
3. Trap. Synchronous internal interruption made by the processor.

If the processor's design incorporates a dedicated pin for interrupts, there needs to be a mechanism for identifying the source of the interrupt (where it originated). This is crucial for scenarios where multiple devices and peripherals share interrupts. The processor must have the capability to discern which specific device triggered the interrupt, enabling it to process the interruption accordingly. This identification mechanism is essential for efficient and precise handling of interrupt-driven events in a system.

Interruptions has “**attributes**” to determine its priority, disabling state, edge sensibility, level sensibility, and other custom “attributes” that are based in the specific processor.

# Priorities

In situations where multiple interrupts occur simultaneously, processors typically have a priority scheme among their interruptions. This priority system allows the processor to determine the order in which interruptions are dispatched. By assigning priorities to different interrupts, the processor can decide which one to address first, ensuring a structured and controlled handling of concurrent interrupt requests. This prioritization is essential for managing the timely and efficient processing of various events in a system with multiple interrupt sources.

