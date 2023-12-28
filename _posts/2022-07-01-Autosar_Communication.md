---
layout: post
title: Autosar Communication
excerpt: Autosar Communication stack
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Communication
  - Automotive
comments: true
category: blog
---

# Introduction
Communication services are a group of modules for all vehicle network communication (CAN, Ethernet, FlexRay, etc). These services interface with HW abstraction and diagnostics BSW modules. They also provide communication management services for SWCs and BSW.

# PDU and Signals
Software Components communicate with lower lever modules (BSW) or between other SWCs at the same level using **RTE** (Run-Time Environment). RTE communication is based on “signals” or “signal groups”. Signals are basic communication objects, and signal groups are complex signal structures. Configuration usage of the **signals** and **signals groups** is dependent on the tool and features. For instance, when E2E transformation is enabled, only signals designated for use by the node (ECU), based on the communication input definition of that specific node, will be protected with E2E.

Communication from COM to lower modules is done through PDUs (Protocol Data Units). A PDU is the basic generic data transfer unit for different vehicle network communication protocols (CAN, FlexRay, LIN, etc). The signals at these protocols are coupled together to the data segment of a PDU.

**PDU** (Protocol Data Unit) is packaged within 2 segments: **PCI** (Protocol Control Information) and SDU (Service Data Unit). **SDU** can be interpreted as low levels PDU which does not contain any header information. SDU is the abstraction for modules beneath PDUR (PDU Router) whereas, for upper levels of PDUR, the PDI is excluded to process only the data structure.
![01](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/30bb01a7-2785-4632-988d-2b84278b1365)

The names of the PDUs depend on what level of abstraction layers are being processed:

    I-PDU in the service layers.
    N-PDU in the network communication layers (ECU Abstract)
    L-PDU in the data link layers.
    IF-PDU used for interfaces between modules.
    TP-PDU used in transport protocols.

The flow of a non-diagnostic signal is as follows:

    The signal is transmitted from RTE to COM through Com_SendSignal ()
    COM packages the signals inside an I-PDU, saving it in an internal buffer and passing it to PDUR using PduR_ComTransmit ()
    PDUR routes the I-PDU to the Transport Layer modules of the selected network communication protocol (CAN, LIN, FlexRay, Ethernet).
    
![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/1d9dbfa2-07bc-4d2e-899b-6f160f6beb6b)

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/c43a458a-854f-4cee-a627-2957e219e3bc)

