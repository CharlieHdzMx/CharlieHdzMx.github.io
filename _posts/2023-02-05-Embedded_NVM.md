---
layout: post
title: Nvm
excerpt: Nvm for Embedded Software
modified: 5/07/2021, 9:00:24
tags:
  - C
  - NVM
  - Embedded Software
comments: true
category: blog
---
NVM stands for Nonvolatile Memory; it is a subset of memory types that retains its value even when the power supply of the chip gets lost.

The size of the smallest erasable unit in NVM memory, known as the NVM cell, varies depending on the type of NVM. NVM cells can be modified either by the application software or by a "tester" device. In cases where the application has the flexibility to modify the NVM at any time, the data must be stored in a Functionally Dynamic NVM, resembling an EEPROM-like device. Conversely, if the NVM can only be modified by a tester through the uploading of "configuration files" into Flash, these configurations typically represent compiler constants.

# NVM blocks
In NVM memory management, data is organized and stored in blocks. NVM blocks refer to instances of NVM memory that encapsulate a defined set of data chosen by the developer to be logically grouped together.

When managing NVM memory, each NVM block can be endowed with various attributes, facilitating effective control and utilization. These attributes include:
1. **RAM Image:**
   - NVM blocks can have a RAM image, simplifying access for read and write operations.
2. **Security Measures:**
   - If a NVM block has a RAM image, the application can secure this RAM image through periodic checks against a predefined content.
3. **Consistency Guarantee:**
   - Each NVM block can ensure consistency through CRC16 or CRC32, providing a reliable means of verifying data integrity.
4. **Data Redundancy:**
   - NVM blocks may incorporate data redundancy, where multiple instances of data items serve as backups in case of invalidation. This is crucial for Functional Safety modules, offering protection against potential contradictions in the event of interruptions during modification.
5. **Priority:**
   - NVM blocks can be assigned priority levels, determining their precedence over others.
6. **Write Protection:**
   - Write protection can be defined for each NVM block, controlling the ability to modify its content.
7. **Mapping Changes:**
   - In the event of software mapping changes over the NVM, certain NVM blocks can be configured to maintain their values during these changes.
8. **Dataset Storage:**
   - NVM blocks can be organized into datasets, allowing developers to structure heterogeneous data for specific functionality access.

These attributes contribute to a flexible and secure management system for NVM memory, catering to diverse application requirements.

The NVM block can be stored in multiple locations for versatile functionality:

1. **Primary Location (NVM):**
   - The primary storage in the NVM provides the main repository for the NVM block.
2. **RAM Image:**
   - A RAM image is maintained for easy access by the application, ensuring synchronization with the primary NVM location.
3. **ROM (Read-Only Memory):**
   - Optionally, the NVM block can be stored in ROM, serving as a repository for default values that are exclusively accessed by the application. This ROM storage is particularly useful for restoring NVM blocks during system initialization.
  
## NVM Block Sizes
In practical terms, opting for large NVM blocks may not be advisable. For instance:

1. **Priority Impact:**
   - Large NVM blocks with lower priority can potentially delay the writing of higher-priority NVM blocks, leading to critical system failures.
2. **Efficiency Concerns:**
   - Large NVM blocks may cause inefficiencies, especially when only a few bytes within the block undergo changes. This results in the entire block being refreshed, impacting system performance.
3. **Page Swap Challenges:**
   - If pages are used to emulate EEPROM in Flash, excessively large NVM blocks can lead to frequent page swaps. This is akin to fitting an elephant in a small room, as it increases the frequency of page swaps, directly affecting CPU load.
4. **Balance with Size:**
   - On the flip side, excessively small NVM blocks may lead to an increase in NVM configuration tables, necessitating careful consideration to strike a balance.

In summary, it's essential to find a suitable size for NVM blocks, avoiding extremes to ensure optimal system performance, efficient use of memory, and minimal impact on CPU load.

# Functional Dynamic NVM corruption
Functionally dynamic NVM, under the control of the application, allows anytime access. This type of storage is typically implemented on **EEPROM**, providing the application with flexibility for dynamic modifications as needed.

Managing this type of NVM introduces several concerns, starting with a clear awareness of how NVM responds to power supply voltage fluctuations.

Certain systems must endure temporary power outages without turning off and initiating a reset. During this survival period, it's crucial to promptly identify and report the outage, taking actions to prevent damage to components and possibly extending the survival time.

Consider defining a strategy to regulate the **writing of NVM blocks based on the power supply voltage**. Establishing an "upper limit" range, for instance, 10 V, where NVM modifications are permitted can be beneficial. Even if your system can tolerate voltages above 6 V, restricting NVM modification at 10 V prevents corruption during rapid voltage decreases to nonfunctional levels. This approach provides the system with adequate time to complete NVM block writing. Notably, it reinforces the recommendation to avoid excessively large NVM blocks, as smaller blocks ensure more efficient writing over time.

Another valuable approach is to notify the **completion of NVM block modifications**, allowing for the determination of a completed NVM update. Multiple protections, such as redundancy, can be in place, ensuring the hardware supplies enough energy. This concept, termed coherency, typically involves using CRC-16 or CRC-32 to compare the target and source data, ensuring correct transmission or storage. Depending on your strategy, alternatives like flags, simple checksums, or sequential numbers may be employed instead of CRCs.

The **NVM manager** plays a crucial role in maintaining the microcontroller's state until NVM updating is complete. For instance, the NVM manager can halt the reset, verify ongoing NVM updating, then power down the microcontroller,and finally allowing the reporting the reset. 

Updating NVM with **priorities** is also a responsibility of the NVM manager. Developers should organize NVM blocks based on their importance. This prioritization strategy ensures the swift updating of critical NVM blocks, providing flexibility to discard less vital information. It's important to note that this strategy works in tandem with other established strategies for effective NVM management. This strategy is performed based on tasks of single NVM blocks updating to reduce the CPU load. For example, the refreshing of the block from a low priority peripheral can be updated in low frequency (an event or 160 mS for example). Updating NVM with priorities falls under the purview of the NVM manager. Developers are tasked with organizing NVM blocks according to their importance. This prioritization strategy ensures the rapid updating of critical NVM blocks while allowing for the flexibility to discard less crucial information. It's crucial to emphasize that this strategy operates synergistically with other established approaches for effective NVM management. Furthermore, the strategy is executed based on tasks of single NVM block updating, aiming to minimize CPU load. For instance, refreshing a block from a low-priority peripheral may occur at a low frequency, such as an event-triggered or 160-millisecond update cycle.

There is the importance of monitoring memory for wearing out. NVM has a limited lifetime, and frequent modifications to NVM cells can lead to malfunctioning, where the cells may start returning invalid values when read. To prevent this wearing out, it's crucial to understand how many times a specific item is modified and assess whether it can endure for the entire system's lifetime. Moreover, there are strategies to prolong the lifetime of data in an NVM cell. One useful approach is the use of multiple buffers to detect signs of wearing out  and determine if a particular NVM cell is reaching the end of its lifespan. This proactive approach allows for effective management of NVM failures, contributing to the overall reliability and longevity of the system.

During the transmission of data through a specific medium, there is a possibility that the data may be damaged or corrupted. To ensure the integrity of the data, various validation methods are employed. These methods include **CRC** (Cyclic Redundancy Check), **Hamming bits**, or **RAID 5**. The choice of validation method depends on the level of robustness that the system requires. Each method has its strengths and trade-offs, and selecting the appropriate one is crucial for maintaining data reliability in the face of potential errors during transmission.

When opting to use checksums in the microcontroller, it's important to consider using them in small units of NVM. NVM blocks should be defined based on their specific functionality. Using checksums in smaller units is advantageous because a simple mismatch of a few bits will only impact the specific NVM block, avoiding the throwing of the entire unit of NVM. This approach has short-term benefits by preventing a direct impact on CPU load and long-term advantages by mitigating the wearing out of NVM cells. It enhances the efficiency of error detection and correction strategies while minimizing the potential broader consequences of errors in larger NVM units.

Validation of NVM is typically necessary during specific events such as microcontroller resets or whenever there are changes in the physical NVM or the **shadow RAM** (if applicable). Regarding the shadow RAM, it's essential to refresh it during every microcontroller initialization and periodically based on the functionality of the related NVM block. In cases where redundancy is employed for NVM data validation, using three copies of the same data is often a prudent strategy, if the NVM size permits. This approach is advantageous because if it detects that two copies are similar but the third one is corrupted, the system can refresh only the corrupted NVM copy. This is more efficient than refreshing all NVM copies, as is the case when using only two copies. It provides a finer level of granularity in error correction, reducing the impact on the system's resources.

