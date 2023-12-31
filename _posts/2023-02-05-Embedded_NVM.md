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
