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

- I-PDU in the service layers.
- N-PDU in the network communication layers (ECU Abstract)
- L-PDU in the data link layers.
- IF-PDU used for interfaces between modules.
- TP-PDU used in transport protocols.

The flow of a non-diagnostic signal is as follows:

- The signal is transmitted from RTE to COM through Com_SendSignal ()
- COM packages the signals inside an I-PDU, saving it in an internal buffer and passing it to PDUR using PduR_ComTransmit ()
- PDUR routes the I-PDU to the Transport Layer modules of the selected network communication protocol (CAN, LIN, FlexRay, Ethernet).
    
![02](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/1d9dbfa2-07bc-4d2e-899b-6f160f6beb6b)

# COM services modules
The communications cluster is made up of modules defined to be independent of the vehicle network communication that is being used, while there are other modules dependent on such vehicle network communications. These vehicle network communication dependent modules will overload the <Bus> spaces with generic interfaces to communicate with other modules independently of its real network communication implementation.

The following image shows the communication service modules, <Bus> modules depend of the vehicle network protocol (CAN, LIN, Ethernet, Flex Ray) :

![03](https://github.com/CharlieHdzMx/CharlieHdzMx.github.io/assets/6202653/c43a458a-854f-4cee-a627-2957e219e3bc)

## SOME / IP Transformer
Some/IP transformer (SOME / IPXF) is part of a chain of transformations that is run by RTE. When an SWC initiates inter-RTE communication, the SWC delivers the data to the RTE, RTE then passes this data to the transformation chain to transfer this data to the communication service cluster. SOME / IP serializes data when it comes from SWCs and deserializes when data goes to SWCs. During this serialization/deserialization, SOME / IP can throw and alert about transformation errors to COM.

##COM-based Transformer
COM-based transformer (COMXF) is based on the COM settings of how the data will be handled. If the configuration demands it, the COM-based transformer can be deactivated or activated. The COM-based transformer converts data from SWCs in uint8 arrangements to forward to COM. Conversely, a COM-based transformer converts the array from uint8 to its original data structure.

## E2E Transformer
E2E Transformer (E2EXF) is used to protect data elements that are related to Functional Safety. When an SWC provides data to be sent to another ECU, then E2EXF adds a mechanism of data protection to the data. When an SWC receives data from other ECU, then E2EXF checks the data for safety protection and outputs the result to the SWC and BSW.

## Secure Onboard Communication
The Secure Onboard Communication (SecOC) module has authentication and freshness mechanisms for sensitive data in PDUs transmission from ECU to ECU. SecOC has support for symmetric or asymmetric mechanisms that use MACs (Message Authentication Code) and digital signatures respectively. When a PDU is sent, SecOC creates a Secured I-PDU by adding an authenticator (MAC or digital signatures) and depending on the Freshness revision method, additionally the Freshness value is included in the PDU payload.

When a PDU is received, the receiver’s SecOC checks the freshness and authenticity of the Secure I-PDU by reviewing the authenticity information previously set by private/public keys.

SecOC serves PDUR with the results of the security processing so that PDUR routes to the specific module.

SecOC depends on CSM (Crypto State Manager) to use cryptographic algorithms, as well as, the signatures and MACs defined in Supervised Entities.

SecOC depends on RTE to be able to call its main functions for Rx and Tx using the BSW Scheduler.

## IPDU Multiplexer
IPDU Multiplexer (IpduM) is used to multiplex IPDUs with static relation, for example, IPDUS with the same IPDU-ID or grouping different I-PDUs within a large PDU to take advantage of the bandwidth of some bus systems that allow larger frame sizes. IpduM is an optional module and can be omitted by configurations.

IpduM depends on COM settings to determine a “selector field”. The field selector defines how the multiplexed I-PDU is to be defined in the static and dynamic parts of the multiplexed I-PDU.

IpduM and PDUR depend on each other for transmission status-based processing of multiplexed I-PDUs.

## PDUR
PDU Router (PDUR) is between the transport protocol modules (lower modules) and higher modules (COM and DCM). IPDUM extends PDUs to transmit multiplexed I-PDUs.

PDUR receives I-PDUs and forwards them to specific higher modules, as well as PDUR transmits I-PDUs based on the request from the higher modules. In addition to this, PDUR serves as a gateway that receives I-PDUs from a transport protocol module and transmits it to the same type of module or other transport protocol modules.

PDUR can receive also N-PDUs frames, either First Frame (FF) or Single Frame (SF). When the last frame is received, the transport protocol module will indicate to PDUR that it is the last frame, so PDUR will send the complete I-PDU to superior modules.

When PDUR transmits an I-PDU, the lower modules will immediately notify (asynchronously) that the I-PDU was transmitted, then PDUR must forward this notification to the higher modules.

In transmission/reception, PDUR is not responsible for returning any errors, this is redirected to the upper modules.

PDUR does not support mechanisms for signal extraction, signal conversion, data integrity checking, or decision making based on the PDU payload.

## Large Data COM
LDCOM enables communication of large data efficiently. LDCOM does not contain the vast majority of the features that COM contains (timeout monitoring, Rx / Tx filtering, signal invalidation, etc), instead it focuses itself on long data transmission from SOME / IP.

## COM
Communication performs the communication of signals from RTE to BSW modules using I-PDUs. It also performs signal routing and signal groups of received I-PDUs.

## Diagnostic Communication Manager
The Diagnostic Communication Manager (DCM) is responsible for mediating the execution of diagnostic services required by external testers through vehicle network communication (CAN, Ethernet, etc).

## Diagnostic Log and Trace
Diagnostic Log and Trace (DLT) provides basic logging and tracing for SWC and internal BSW modules (RTE, DET, and DEM). An SWC can call DLT to send its status information using a log or trace, DLT receives this information and converts it into standardized formats for transmission to other BSW modules.

## Generic Network Manager Interface
The Generic Network Manager Interface (NM) is required to run the NMs of the specific network buses and to be able to synchronously turn off several buses at the same time.

# <Bus Specific> Communication Service module

Every Bus (CAN, FlexRay, LIN, Ethernet, etc) has a dedicated Network Manager (NM), Transport Protocol (TP), and State Manager (SM) modules. These modules are self-sufficient and only depend on their own communication system.

Bus-specific NMs coordinate periodic PDU reception/transmission algorithms. SMs control the communication flow abstraction of their specific bus. TPs contain information on how to process the transmission, reception, and segmentation of PDUs on your specific bus.

Each bus contains a set of peculiarities:

- CAN specific modules contain TTCAN and J1939 extensions. In the CAN stack, TTCAN and J1939 add interfaces and driver support. In the case of J1939, a J1939TP is used instead of the CANTP.
- LIN specific modules contain support for schedule tables using a schedule manager. These modules also contain interfaces to control the LIN Sleep mode and Start / Stop features. Some LIN slaves would not be able to support the Autosar architecture, so they are defined as complete non-Autosar compliance ECUs by the system.
- FlexRay modules define 2 types of transport layer, FrTp, and FrArTp (FlexRay Autosar).
- TCP / IP modules implement the TCP / IP family of protocols: TCP, UDP, IPv4, IPv6, ARP, ICMP, NDC, DHCP). TCP / IP uses the Socket Adapter (SoAd). SOAD converts TCP / IP socket-oriented communication to PDU-oriented communication.
