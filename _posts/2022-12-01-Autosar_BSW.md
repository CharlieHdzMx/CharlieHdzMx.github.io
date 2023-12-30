---
layout: post
title: Autosar BSW
excerpt: Autosar BSW
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - BSW
comments: true
category: blog
---
BSW is a standardized layer of services that serve as an interface between the HW and SWCs. BSW comprises the service layer abstraction layer ECU abstraction layer and the uC; as well as a cluster of complex drivers.

![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/b0032a84-c3e7-43fc-a906-4355aac4fc34)

BSW can be subdivided into the following services that are offered to SWCs:
1. I / O, abstraction of standardized access to sensors, actuators and peripherals of other ECUs
2. Memory, abstraction of standardized access to internal and external memories.
3. Crypto, abstraction of standardized access to drivers, primitive types and cryptographic entities.
4. Communication outside the vehicle, Abstraction of standardized access to Car2-X communication for wireless connection systems.
5. System Offers standardized operating system and ECU-specific services. For example, watchdog manager, OS manager, Timers, Errors and ECU states.
6. Communication ECUs, CAN bus support, LIN, FlexRay, J1939, Ethernet, ect.

BSW will request internal / external resources from the uC through drivers; These drivers
can be internal or external. Examples of internal drivers are EEPROM, CAN / LIN / Ethernet
drivers or ADC samples. Examples of external drivers are SBCs (transceivers and external
watchdogs). Abstractions from internal drivers will be positioned in the microcontroller
abstraction layer, while abstractions from external drivers will be positioned in the ECU
abstraction layer.

# Service Layer
The service layer is the main provider of basic services to application SWCs. The service layer provides operating system functionality, in-vehicle network management and communication services, memory services, diagnostic services and ECU status management.

The service layer contains:
1. **System services**. It is a group of modules that interfaces can be used by any layer for OS status, error handling, diagnostic event handling, watchdog and communication management.
2. **Memory services**. It consists only of the NVRAM Manager which is responsible for the correct writing / reading of non-volatile data from different abstractions of memory drivers.
3. **Communication services**. It is a group of modules for all vehicle network communication (CAN, Ethernet, FlexRay, ect). They interface with HW abstraction and diagnostics; It also provides communication management services for SWCs and BSW.
4. **Crypto Services**. Mainly based by the Crypto Service Manager (CSM) who is responsible for handling crypto jobs and key storage. CSM Jobs is an abstraction of CMS keys, CMS queues, and primitive CSM types.

# ECU Abstraction Layer.
The ECU abstraction layer interfaces all drivers from the uC abstraction layer to the services layer. It also contains drivers external to the uC. It abstracts all access to peripherals (whether internal / external to the uC). The ECU abstraction layer is made up of the following clusters:
1. Onboard Device Abstraction, contains abstractions of drivers for onboard ECU devices that cannot be considered sensors or actuators (watchdogs for example).
2. Memory Hardware Abstraction, contains abstractions of peripheral memory devices, either internal or external. Memory drivers are accessed through abstraction and emulation modules. By emulating the EEPROM on top of the Flash hardware, a general memory path is enabled for use as an abstraction.
3. Crypto Hardware Abstraction, is a group of modules that abstract through internal access mechanisms to primitives from internal cryptography or from external devices.
4. Wireless Communication HW Abstraction, contain abstractions for wireless communication.
5. Communication Hardware Abstraction, is a group of modules that abstract through access mechanisms to bus channels regardless of their accommodation (on-chip / onboard).
6. I / O Hardware Abstraction, is a group of modules that abstract I / O devices (onboard or on-chip) through a representation of I / O signals that are connected to the hw of the ECU (current, frequency, voltage). This abstraction does not abstract sensors and actuators.

# uC Abstraction Layer
The microcontroller’s abstraction layer is the lowest SW layer in BSW. It contains all internal uC drivers, therefore, with direct access to the microcontroller and its internal peripherals. Its main objective is to make the upper layers independent of uC. The uC abstraction layer is made up of the following clusters:
1. Microcontroller drivers.
2. Memory drivers
3. Crypto drivers
4. Wireless communication drivers
5. Communication drivers
6. I / O drivers

# Complex Drivers
The complex driver layer is a direct extension of the drivers to RTE. It provides the option to integrate special purpose functionalities that are not specified by Autosar, have high time limits or come from a migration.

# RTE
RTE is the layer that provides application SWCs with communication with BSW, intra-ECU SWCs or inter-ECU SWCs. RTE makes SWCs independent of ECU mapping. When a SWC want to send data to the ECU, RTE can handle data in a buffer in-place, or not:
1. RTE calls the SOME / IP Transformer hosted internally on RTE.
2. SOME / IP Transformer converts the data to an byte array and saves it in a buffer. This byte array is used by a Safe Transformer to add data security mechanisms. Depending on whether or not an in-place buffer is configured, an additional buffer is used to keep the data transformed.
3. RTE transfers the transformed data to COM.
4. COM serializes data from RTE as configured (communication matrix).
   
![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/ff0b14d3-1867-4bbc-a810-29c3750fc5f8)


