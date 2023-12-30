---
layout: post
title: Autosar Memory Stack
excerpt: Autosar Memory Stack
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Memory
comments: true
category: blog
---
Non-volatile data sets are required to be persistent for a variety of reasons. For example, some DTCs and Vehicle calibrations are required to “survive” ECUs resets to be able to communicate with other ECUs or external tools properly.

Autosar architecture provides the “NVM” stack to provide mechanisms to store and maintain non-volatile data and permit access to different levels of SW. The NVM stack involves the following BSW modules:
1. **NVRAM Manager** (NvM) accesses devices drivers via index abstractions to provide services that ensure the required synch/async storage and maintenance of each individual NV data.
2. **CRC** contains different CRC routines that are used by NvM to check and generate CRC to be used by the NVRAM verification option.
3. **Memory Abstraction Interface** (Memif) allows the NvM to access several abstractions of the memory models either by EEPROM abstractions (EA) or Flash EEPROM emulations (FEE).
4. **EEPROM Abstraction** (EA) and **Flash EEPROM Emulation** (FEE) provide upper layers with 32-bits “virtual” linear addressing scheme and segmentation for internal functions from the uC abstraction layer modules or for external EEPROM/flash devices.
5. **EEPROM driver** (EEP) access the HW directly to provide asynchronous functions to write, read, and erase to/from internal EEPROM/Flash.
6. **Ram Test** (RamTst) tests the physical health of RAM/Flash cells.
7. **Flash Test**(FlshTst) tests invariable memory such as data/program flash, SRAM, memory-mapped to the microcontroller resources.

External EEPROMs can be emulated by on-chip flash memory. The methods to emulate EEPROM requires the use of “page swaps” to transfer the data from one page to another when there is no space left for adding more NVM blocks; usually, 2 pages are used on emulated EEPROMs and the page swaps are performed as a background task or low priority task.

All ECU abstraction modules and microcontroller abstraction modules are associated with the Diagnostic modules (DEM, DET, etc) by default to be able to report or write errors across the complete system.

# NVRAM Manager 
**NVRAM Manager** (NvM) is the unique service module that permits application SW access to non-volatile memory blocks. NvM receives interfaces of virtual linear 32 bits addresses with are divided into one subset of 16 bits related to logical block number (block identifier and Bit selection) and another subset of 16 bits related to the address offset.

NvM accesses the data by asynchronous and synchronous services depending on the type of basic storage objects. The basic storage object is the smallest abstract entity of the NVRAM block that allows the handling of non-volatile memory by NvM. There are different concrete types of basic storage objects.
1. **NV Block** represents a memory area consisting of NV user data and optionally a CRC value. NV blocks are usually used to hold the application data and are a mandatory part of NVRAM blocks.<img2>
2. **RAM Block** represents a RAM area consisting of user data and optionally a CRC value. The RAM block data can be either temporary or permanent, if the RAM block data is defined as permanent, then the data is known at compilation time, otherwise, the RAM block is known until run-time. The decision to use permanent or temporary RAM data is based on whether or not the RAM block is dedicated only by one
SWC/BSW-module, or can be shared among various SWCs/BSW-modules. The RAM data of this block can later reside in the global RAM area as well.
3. **ROM Block** resides in the ROM/FLASH and is used to provide default values when an NV block was damaged or non- initialized correctly.
4. **Administrative Block** resides in non-persistent RAM and contains an index to be associated with other NV blocks. Administrative Block contains also attributes, errors, and status of the associated NV block, for example, Administrative Block contains whether or not one permanent RAM block is invalid or not, or if one NV block is write-protected or not.

NvM supports predefined NVRAM management types that consist of different NV blocks, for example:
1. **“Native” NVM block** is the simplest block management type that contains one NV block, RAM image block, an Administrative block, and optionally a ROM block for
default values.
2. **Redundant NVM** block is the same as the Native NVM block but might contain extra NV blocks to enhance fault tolerance, reliability, and availability (important for safety/security features).
3. Data Set NVM block is an array of equally-sized NV blocks that depend on the configuration of NvmDatasetSelectionBits and that are reference by one RAM block and one Administrative block, allowing only one element to be addressed by ASW at the time. If default ROM blocks are selected by configuration, the index of all elements NV blocks is used to indexing the ROM blocks.

NvM provides, by configuration, callback interfaces to respond for asynchronous request resolutions (write, read or error responses). NvM is also allowed to queue access requests of one specific NV block (addressed by the block ID) from different modules.

## NvM Startup/Shutdown, Consistency and Error Recovery
NvM is not responsible for neither initialize the NVRAM blocks nor trigger the initialization of memory drivers. The initialization of NVRAM blocks is performed by the ECU state manager by the call of NvM_ReadAll. During startup, the RAM blocks are implicitly “recovered” using their associated ROM default data. The ROM default data is loaded if the RAM block was invalid, the CRC was inconsistent, and the NV block state was invalid too.

The shutdown procedure of NVRAM blocks is done by the ECU state manager which invokes the request of NvM_WriteAll. This request is based on asynchronous interfaces that queue the block by priority.

NvM is responsible to provide implicit techniques to check the consistency of the data of NVRAM blocks; this consistency check is done by configurable function calls of CRC recalculations.

The write error recovery of the NVRAM block is independent of the NVRAM block management type, whereas, the read error recovery depends on the NVRAM block management type. For example, if the NVRAM block management type is REDUNDANT, then the recovery is done by loading the redundant NV block data with default values. The NvM is incapable to detect incomplete write operations of any NV block. This is
responsible for the modules of the hardware abstraction. NvM only receives notification information if one NV block is invalid or inconsistent.

The asynchronous request from NvM can indicate the result on the administrative block (error/status) for both single block requests and multiple block requests. Optionally, there is the configuration of NvmMultiBlockCallback that notify the termination of a multiple block request using callbacks.

# NvM communication with ASW
Nvm can’t ensure the data integrity of the RAM block, therefore the multiples SWCs that are using the same RAM block have the flexibility to use complex synchronization mechanisms. This approach enables guaranteed synchronization and data corruption avoidance.

For example, the SWC write request can follow the following steps:
1. The application requests a RAM block filling to NvM with the data that should be written using the Nvm_WriteBlock request.
2. The application shall not modify the RAM block (only read it) until the RAM block is signaled as a success or failure.
3. The application during the meantime can also ask for the status of the request by polling or by asynchronous callbacks.
4. The application also can cancel the write request by calling NvMCancelWriteAll.
5. After the NvM signals the RAM block writes as complete, this RAM block is reusable again for modifications.
   
The SWC read request can follow the following steps:
1. The application issues the Nvm_ReadBlock request to transfer control to NvM in order to process a RAM block that has to be filled with NVRAM data from NvM.
2. The application shall wait until the request result is signaled (either success or failure).
3. The application during the meantime can also ask for the status of the request by polling or by asynchronous callbacks.
4. After the completion of the NvM operation, the selected RAM block is available to be used by an application with new data.

Multi blocks reading and writing are triggered only by the ECU state manager at system startup. Usually, these request fills the permanent RAM blocks with data that is required to initialize the ECU. ECU State manager requests these multiblock reading and writing by using the request of Nvm_ReadAll and Nvm_WriteAll; also ECU state manager can use either polling or callbacks to get the status of the requests during their process of reading/writing.

# ECU Abstraction Modules
## Memory Abstraction Interface
Memory Abstraction Interface (MemIf) abstract the underlying Flash EEPROM Emulation (FEE) and EEPROM Abstraction (EA) interfaces and provide NvM with virtual segmentations on uniform linear address space. MemIf selects the specified mapped API of the underlying memory abstraction module, either FEE or EA. This selection can be realized by arrays of the pointer to functions where the index is the DeviceIndex. 
DeviceIndex determines the memory abstraction module.

## Flash EEPROM Emulation
FEE will provide to upper layers the **32-bits virtual linear address location**. This 32-bits location is separated into two 16-bits numbers: logical block number and block offset. The logical block number for non-redundancy and no-dataset represents the configurable paging mechanism done by the parameter FeeVirtualPageSize. The logical block shall be multiple-aligned with the address alignment that defines this FeeVirtualPageSize parameter. Following this alignment, logical blocks shall neither overlap nor contain themselves. The 16-bits block offset along with the logical block number is combined by FEE to derive the physical flash address which is used by the underlying flash drivers.

FEE is also capable to define the mechanisms to limit the global number of write access to avoid the overstressing of the physical device if the underlying drivers do not specify a configured number of write cycles per physical memory cell.

FEE shall ensure the instant writing of “immediate” data without the need of erasing the corresponding memory area, even when there are other block-writing requests from NvM in the process. This instant writing is performed by having pre-erased memory areas and is commanded when NvM request the cancelation of the writing of the non-immediate data and the write request of the immediate data that are performed synchronously by FEE (or the underlying driver). Usually, this immediate writing is performed without delay, if there no page writing, erasing of a memory sector, or SPI transference being executed.
FEE manages the correctness and validation of the information of the block too. The block correctness determines if the block is corrupted or not, and FEE marks one block as “corrupted” until the writing process is done, then FEE sets the block as “not corrupted”.

The validation of the block is reviewed by FEE by the service Fee_InvalidateBlock, which determines if the block has been deliberately invalidated by upper layer modules. 

# EEPROM Abstraction
EA depends on NvM to serialize the jobs that are required to EA since EA can only process one job at a time and cannot queue pending jobs. EA, as FEE, does provides to upper layers the **32-bits virtual linear address location.** This 32-bits location is separated into two 16-bits numbers: logical block number and block offset. The logical block number for non-redundancy and no-dataset represents the configurable
paging mechanism done by the parameter EAVirtualPageSize. The logical block shall be multiple-aligned with the address alignment that defines this EAVirtualPageSize parameter.

Following this alignment, logical blocks shall neither overlap nor contain themselves. The 16-bits block offset along with the logical block number is combined by EA to derive the physical flash address which is used by the underlying EEPROM drivers.

The handling of “immediate” valid and correct data writing of EA is similar to FEE handling. Refer to the FEE paragraph about immediate data writing for more information.

# Microcontroller Abstraction Modules
## EEPROM Driver

**EEPROM driver (EEP)** access the HW directly to provide asynchronous functions to write, read, and erase to/from internal EEPROM/Flash. Internal EEPs are located in the microcontroller abstraction layer whereas external EEPs that use SPI handlers are located in the ECU abstraction layer.

Internal EEPROMs are subordinate to the microcontroller system clocks, so any configuration that varies the system clock might affect internal EEPROMs as well. External EEPROMs depend uniquely on the capabilities of the onboard communication handler.

EEP can only dispatch one job at a time. When dispatching a job, EEP can neither buffer jobs nor accept jobs. When processing a job, EEP shall set the EEPROM state to IDLE (indicating that it can process a new job) and set the job to be OK; additionally, EEP can call the notification defined by the configuration EepJobEndNotification. When an error is detected during a write/erase/read/compare job, the job shall be discarded, then EEP shall set the EEPROM state to IDLE but the job shall be set to be FAILED, and (if configured) call the failure notification.

EEP is able to activate or deactivate development error detection by the parameter EepDevErrorDetect, but EEP shall not deactivate the production code error detection.
