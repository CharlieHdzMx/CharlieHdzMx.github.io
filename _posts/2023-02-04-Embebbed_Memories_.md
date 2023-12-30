---
layout: post
title: Embedded Memories
excerpt: Memory types definition for embedded software.
modified: 5/07/2021, 9:00:24
tags:
  - Qt
  - Cpp
comments: true
category: blog
---
If you came with a System or IT perspective, you might only acknowledge two types of memory ROM and RAM, but there are sub groups from these memory groups and actually a merged between these two types of memories which is called hybrid.

For those learning Embedded Systems, a crucial aspect is understanding the distinctions among different types of memory. This awareness becomes essential when making decisions about the appropriate memory for a given application or adapting to an existing project. A solid grasp of memory types empowers you to make well-informed choices, aligning your decisions with the specific requirements and constraints of your embedded system.

The common memory types in embedded systems are:
1. RAM (Random Access Memory)
  DRAM
  SRAM
2. ROM (Read Only Memory)
  ROM
  PROM
  EPROM
3. Hybrid
   NVRAM (KAM)
   Flash
   EEPROM

# RAM memories
RAM memories permits the reading and writing of data in specific memory location, but they cannot retain the data after a power removing. This memory family characteristic is that they are volatile writable memories and their process is byte per byte with unlimited cycles.

The **SRAM** stands for Static RAM; its principal characteristic is that retains the data as long as the power is applied to the chip. The **DRAM** stands for Dynamic RAM, its principal characteristic is that has extremely short retainment of data even when the power is applied to the chip constantly. To make DRAM more reliable, there is a special hardware called DRAM controller. DRAM controller refreshes the DRAM data several times per second to maintain the data alive as it is needed to be.

Comparing SRAM with DRAM, SRAM is faster than DRAM but is more expensive. SRAM is recommended to use in application were the access speed is critical. DRAM is useful for large-size memories where the access is not crucial.

Of course, there are several Embedded Systems that combine SRAM and DRAM into a whole RAM device. SRAM is for important data that needs fast frequent access, while the DRAM contains non-important data that requires no so frequent accesses.

If you are using a DRAM controller, it needs to be started in the first steps of a chip startup, this is because, and maybe your system requires the stack to be located in the DRAM.

# ROM memories
ROM memories permits the reading and constrained writing of data in specific memory location, they can retain the data after a power removing (persistency). All ROM memories are considered slower than RAM memories.

The basic ROM is nonvolatile and not writable memory which is programmed only one by the manufacturer of the chip. After this programming, the user cannot change anything in memory, is already hardwired. Its main advantage is that in high quantities, ROM memories are very cheap, still their speed is slow compared with RAM.

**PROM** stands for Programmable ROM is nonvolatile and only once writing memory. You can buy a PROM with an unprogrammable state and use a programmer to pass your data. Once you program a PROM, it cannot change anymore.

# Hybrid memories
Hybrid memories offer a ROM device with RAM capabilities in certain conditions. These memories can be read and written as required but they are at the core slower than RAM, but their data is persistence after power loss. EEPROM and Flash come from ROM origins while the NVRAM or (KAM) comes from a modified RAM.

**EEPROM** stands for Electrically Erasable and Programmable ROM, which has the capability of several writing while persisting the data after power supply loss, but the problem is that these writing are limited.
Flash combines the best features of all the memories, these memories are very similar than EEPROM but they achieve a faster writing by rewriting by sectors instead of by bytes. They are also cheaper than EEPROM.

EEPROM and Flash as we described early, have a limited number of writings; after a time, their cells starts to fail and sometimes they contain the correct values but sometimes they did not. The number of writing depends of the device but also depends of the other things (data size, frequency of writing, temperature, etc.).

**NVRAM** and **KAM** are special RAMs that maintain their value when power is removed. Those uses a backup battery to maintain the RAM powered. Some of these NVRAM are highly dependent on the RAM design and  anufacturing so usually they are avoided. KAMs that have their battery mounted in the PCB are the most readable memory among NVRAMs. NVRAM are also expensive and fast.
