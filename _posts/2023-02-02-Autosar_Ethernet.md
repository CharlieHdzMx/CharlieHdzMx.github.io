---
layout: post
title: Autosar Ethernet
excerpt: Autosar Ethernet
modified: 5/07/2021, 9:00:24
tags:
  - Autosar
  - Ethernet
comments: true
category: blog
---
One of the principal reasons that automobiles are embracing ethernet communication is due to the tendency that **distributed open systems** are becoming more frequent inside the vehicles. Distributed systems mean the distribution of vehicle functionalities into different ECUs (being more and smaller as time passed), these distributed systems are demanded principally to high-speed networks, for RTOS systems, and special safety requirements. Open systems are systems that actively interact with external systems (Infotairment interfaces or electric vehicle charge stations); an open system requires additional measurements of communication and behavior security. Additionally, Ethernet integration adds advantages to the systems such as big SW packages download for updating purposes, big data transferences, and flexible data communication.

Ethernet is used for sophisticated architectures where other vehicle communication networks are unable to meet the requirements (CAN, LIN, FD, etc); this is due Ethernet is cheaper, faster, and more secured that these vehicle communication networks.

Ethernet integrates a gateway to have a dedicated link of nodes. Ethernet cannot have data collisions due to the switching of communication of the nodes only permits the communication between the sender and the receiver.

Ethernet is based on the vehicle usage of the OSI model (differing only in the application layer and being the same at the physical and network-level). Ethernet was introduced in Autosar 4.0 with increasing support as newer versions continue to be released.

The scalibility classes of the Autosar 4.2.1 protocol refering to Ethernet are:
1. IPv4: ARP, ICMPv4, and DHCPv4.
2. IPv6: NDP, ICMPv6, and DHCPv6
3. Dual-stack: containing both IPv4 and IPv6.

# Ethernet Basics
## Physical Layer
To be able to communicate through Ethernet, the microcontroller requires an **Ethernet MAC**. This communication with MACs is performed by a **PHY transceiver** (physical layer) through the MII interface (Medium-independent interface). MII is specialized in the connection of different physical layers with different MACs, to transmit from a PHY to the physical medium of another ECU, the MDI interface(Medium-Dependent Interface) is used.
<Fig 1>

## Data Link Layer
The **data link layer** defines the basic functionality of communication: Bus access, frame formatting, and how the node direction is done. The node direction can be unicast (1:1), multicast (1:m<n), or broadcast (1:n); where m is a preselected subset of nodes in the network, and n is the total of nodes available in the network. The data link layer is subdivided into 2 sub-control layers, the Logic Link Control (LCC) and the **Medium Access Control** (MAC). LCC controls the different connections with the use of high layers abstraction, whereas MAC provides direct access to the physical layer (within access to the bus, frame formatting, and node addressing).

Inside an Ethernet network, each ECU shall have a unique MAC address and a PHY transceiver for its own use. For example, when ECU-A is trying to communicate with ECU-B, the first step is that ECU-A identifies itself by its MAC address, and after the identification is done, ECU-A would send an ETH frame (data) to ECU-B.
As a general overview, the ETH frame contains:
The preamble to synchronize the network timers.
Destiny address.
Source address.
Frame type.
Payload content.
CRC.
Node Switching
The way that the switch links the MAC with the PHY transceivers is by using an associative table that is predefined using a PHY identifier; this PHY identifier is saved at the first reception of the ECU ETH frame. In the image below, the switch broadcasts the ECU1 frame where ECU3 would respond passing through the switch with the Destiny Address of ECUA; when this bidirectional communication is done, the switch inhibits the broadcasting of the initial ECUA frame and set a communication link to continue the transmission of data between ECUA and ECUB.
<Figura 2>
If one ECU does not recognize the destination MAC address, the destination address can be recovered by protocols from the upper abstract layer – ARP or NDP. If ARP or NDP are unable to discover the destination ECU address, then the request is moved to an even higher abstract layer to still try to discover the service medium.
There are no common use cases in the automotive industry to configure the Ethernet network to be dynamic, therefore, there is no usual need to handle the disconnecting of ECU from the Ethernet network.
VLAN enables the network division into different sub-networks. To perform the sub-network division, the ETH frame contains a VLAN tag that shall be used by the switch (VLAN+MAC) to be able to identify the ECUs that are assigned to this sub-network; this ECUs identification is done by sending broadcasting messages but ignoring the responses from every ECU that is not part of the VLAN sub-network.
The network switches for Automotives can have different configurations. For example, there might be switches that integrate a MAC driver along with the switch engine (either independent MACs connected directly to each PHY or the switch engine connected to every PHY), or that the microcontroller is connected with an external IC (containing an integrated MAC) that contains an Ethernet switch and that the control communication is performed by a simpler standard communication (SPI).
<Figure 3>
Network Layer
The network layer permits data package routing through the network limits. The TYPE part of an ETH frame identifies the Ethernet protocol is going to be used: if the TYPE is 0x0800, then the network is IPv4 and if the TYPE is 0x86DD, then the network is IPv6. The network layer is used by upper protocols such as TCP and UDP, as well as, other abstracted mechanisms.
When a network layer abstraction is used, there would be an IP address divided by the following sub-sets:
The network prefix, the first 4 bytes of an ECU/Router address.
The ID interface, the last 4 bytes of the ECU/Router address.
A router maintains the context and semantic of the IpxV that is being used, also of the MACs and independent PHYs for each network abstraction. Each PHY of a router also has an IP address for each network abstraction.
A network abstraction serves to exclude the ID interface from the network prefix. The CIDR (Classless Inter-Domain Routing) is a way to interpret the IP addresses through the bit numbers in a netmask and being written from left to right. Each network abstraction can contain 254 nodes, the interface IDs on network abstraction starts from 1, for the router, and subsequent number for the next nodes. The switches abstraction from this layer defines which LAN is going to be used, while the router connects different VLANs together. For the next figure, the router can communicate ECU-A with ECU-C by the MACs discovering.
<Figure 4>
The major difference between IPv4 and IPv6 is that IPv6 can extend the number of local and global addresses (possible ECUs to be connected). The interface ID for IPv6 is 64 bits whereas IPv4 is 8 bits. In IPv4, it is possible to define network classes that can be defined as private and cannot be accessed by ECUs that are not in the corresponding local network. Moreover, IPv4 and IPv6 define an address range called loopback that streams the received frame and transmit it again; this range is usually used in debugging and test scenarios. 
IPv6 extended address range is defined as hexadecimal sub-range as follow:
Zero Address. Addresses that contain 0s as offsets.
Loopback.
Local link address. Private communication from VLAN switches.
Unique local Unicast
Multicast
Global unicast.
Address Resolution Protocols (ARP) with IPv4
The Address Resolution Protocol (ARP) can find an unknown MAC address through an already known IP address. The ARP uses ETH frame 0x0806 to broadcast a MAC address to the destination IP. If the MAC address from the ARP request is respondent by an ARP reply, then this ECU would contain the MAC address in its ARP cache. ARP does not interface with the network layer abstraction (IPv4).
Neighbor Discovery Protocol (NDP) with IPv6
The Neighbor Discovery Protocol (NDP) can find an unknown MAC address through an already known IP address. The node that requires to detect an unknown MAC address would send its own IP address using a special request called Neighbor Solicitation, and later on, perform a multicast to a specific group of nodes. When one node can respond to this client IP address, then it would send its own MAC address through a special reply called Neighbor Advertisement; to be saved on the Neighbor cache.
Transport Layer
The transport layer performs the communication of application software among ECUs through ports. For example, one application “A” from a port sends an ETH frame with the IP address of the destination ECU, inside this ETH frame, every application that is able to handle this frame is referenced, so when the frame arrives at the destination ECU, the frame is transported to the specific port and it is processed by IP dedicated APIs to be delivered to the application “B” from the destination ECU.
User Datagram Protocol (UDP) is the transport layer protocol that permits the communication of data without segmentation and does not require any acknowledgment for the destination ports. The IP package is composed of the UDP specific header (17), along with the UDP payload. This IP package is sent from an origin port and it is received by the destination port. The principal benefit of UDP is that no connection setup is required and the IP packages are expected to be processed by the receiving application.
Transmission Control Protocol (TCP) is the transport layer protocol that permits the communication of data with segmentation and requires full acknowledgment for the destination ports, as well as, the acceptance of all the IP packages. Usually, TCP permits the sending of multiple IP packages with the same context, in this case, the transmission mode is similar to the described in the UDP section but the header would contain the IP protocol field = 6. TCP permits duplex communication where both ports can communicate bidirectionally and acknowledging other ports when they finished their communication session. The way that TCP transmit data is by a three-way handshake, explained as follows:
First, ECU-A (origin port) sends some DATA-A to ECU-B setting the ACK flag along with a sequential number (SEQ).
ECU-B receives the DATA-A and modifies the SEQ and ACK to demonstrate that the data was received.
Then ECU-B responds with DATA-B.
ECU-A receives DATA-B and proceeds to send the principal data along with an increment of the SEQ and ACK data.
Each time that the receiver obtains data, the ACK data is increased, if data or the ACK are lost then the sender would not receive any ACK reply before the timeout, so the sender would try to transmit again. If multiple segments are sent and one is lost, the ACK would be pointing to the missed one.
TCP also has a mechanism called Sliding Window Protocol. The Sliding Window Protocol shows the number of buffers available in the receiver port to the sender port. In the following example, the receptor notifies that has at least one available buffer (W=1), then, the sender increment SEQ and send data to fill W, during this process, the receiver freed 5 buffers and notify it with W=5 along with the ACK increasing to indicate that the previous data was received correctly. Later, because the receiver acknowledged the previous data, the sender confirm the interruption and deletion of the data previously sent (otherwise, the sender will continue retransmitting this data). In the next sending, the sender would increase SEQ and would send the data that W permits (in this case 5), then the receiver would increase ACK and indicate that it has other 5 available buffers (W=5), then the sender receives ACK, request the removal of data and send the remaining data <= W.
<Figure 5>
TCP congestion control or Slow Start is a TCP mechanism that is relevant only for the vehicle network connections. Slow Start, in addition to the TCP mechanism, would send a frame with the minimum size, if this frame was received correctly, then then the size is duplicated. This duplication will continue until the receiver is not able to accept the last frame size due either to a predesigned threshold (SSTHRESH) or a timeout is expired due to the frame size.
There is a TCP issue called Silly Windows. Silly Windows issue happens when windows are freed and the transmission is done by packages that are too small. This situation causes large communication overhead due to TCP to transmit a lot of secondary data. A way to avoid this Silly Windows issue is by using the Nagle algorithm, which saves the collection of data that are sent only when the amount of data is big enough. If the receiver timeout is near and the data is still not ready to be send, the Nagle algorithm defines the Nagle timer to avoid the receiver timeout to expire.
When the transmission between some nodes are required to be finished, then the sender is the first one to send an Ending request (along with the increment of SEQ and ACK) to indicate that it does not have more data to send. Then the receiver acknowledges the Ending request of the receiver and sends an Ending reply (along with the increment of SEQ and ACK). Still, the communication is not ended yet, the receiver sends a second Ending reply to indicate that the communication will be freed, if the sender process this reply and accept it, then the communication ending acknowledge is sent and finally, all the resources and locks of this communication are freed.
Dynamic Host Configuration Protocol
Dynamic Host Configuration Protocol (DHCP) is the protocol used to automate the IP address configuration, as well as, the subnet mask configuration and other configurations that enable the IP address high-level automated redirection. Usually, IPv4 is used and the client sends a DCHP discovering asking for a particular IP address, then the node network (server) responded with a DCHP offer and the client node receives the offering through a DCHP request.
Autosar Ethernet
The great majority of use cases of TCP/IP are not involved in the automotive industry; moreover, sometimes the embedded system limitations inhibit the use of some TCP/IP protocols. The Ethernet integration shall comply with the Autosar requirements regarding the socket mapping based on the service/signal communication, as well as the ECUs network management.
Sockets are the communication endpoint to receive data. There are sockets to datagrams (UDP) and sockets to streamflow (TCP). The datagram sockets can transmit packages without configuration that are less than 1472 bytes, while a stream socket always requires a connection/disconnection configuration as well as always segment the data to transmit.
<Figure 6>
The socket addressing is handled using a 4-tuple that is an array composed as: {Source IP Address: Source Port, Destination IP address: Destination Port}. This addressing can be dynamic and the data transmission can be dynamic only to datagram sockets (UDP)
To combine the signal/PDU signals together with the Ethernet sockets, the integration of SoAd (Socket Adaptor) is required. SoAd converts the representation of sockets in PDUs and the ETH module( Ethernet driver) is used in the abstraction of PDUs called PDUR(PDU Router).
<Figure 7>
Compared with the other vehicle communication protocols that use PDU, the high magnitude of data transmitted by Ethernet protocol can cause overhead of PDUs. To mitigate this to some degree, PDU containers are implemented, these PDU containers can group PDUs using its dynamic allocation. Together, SoAd and IPDUM (I-PDU Multiplexer) modules integrate the PDU container along with the PDUs header to dynamically identify each PDU from the container.
One use case of PDU containers is when a system is composed of at least an ECU network that communicates through CAN to an external entity, instead of sending information by each-ECU individual connection, any ECU acting as a gateway can be used to collect all the PDUs from each ECU and by configuration determine the trigger to send all of them as a group (not necessary send them when the group is filled out).
The principal logic of PDU containers is located in IPDUM, this module creates abstractions to independently communicate regardless of its type of network protocol. IPDUM shall be focused on sending big chunks of frames to take advance of the inner logic. The mapping of which PDUs are moved to a specific PDU container is static, while the PDU content allocation is dynamic.
There are different ways to trigger the transmission of PDU containers:
The space of the PDU container is less than a specific threshold.
The logic where the PDU container is being processed had reached a timeout.
The PDU located on the PDU container had reached a timeout.
PDUs sending based on priority.
PDUs sending in FIFO format.
The PDUs headers can be of 4 bytes (3 bytes of ID and 1 byte of length), or 8 bytes (4 bytes of ID and 4 bytes of length).
The PDU container concept permits efficient use of the bandwidth and permits the achievement of less overhead to the receivers. The PDU containers can be created by using SoAd or IPDUM, but SoAd offers additional dynamic configuration of communication stack flows. One example of the use of SoAd can be the following image:
<Figure 8>
The ECU sender from the image has 3 configured socket that transmits to ECU receptors (R1, R2, and R3).
The sender sends a PDU from the application, this PDU is stored in socket 1 and 2.
When receiving the sender PDU, a gateway sends an additional PDU to the sender.
The sender receives and processes the PDU which is stored in socket 2 and 3.
Within the 3 PDU, socket 2 reached the configured threshold, then all the container is sent to R2.
Socket 1 reaches a timeout that designates the PDUs sending to R1.
Socket 3 is configured to transmit all the PDU containers to R3 when a PDU type 3 arrives.
Autosar Ethernet Communication Stack
<figure 6 again>
SoAd- Socket Adaptor. Merges the Autosar communication stack below PDUR, SoAd principal function is to convert the communication based on the socket to communication-based on PDUs. SoAd can have different upper modules such as UDPNM (UDP Network Manager), SD (Service Discovery), ETHXCP (Ethernet XCP support), and so on.
TCPIP- TCP/IP. It Contains all the protocols defined on TCP/IP (IPv4, IPv6, ARP, NDP, ICMPv4, ICMPv6) as well as, the transport protocols UDP and TCP.
TLS- Transport Layer Security encrypts and authentificate TCP, UDP cannot be supported by TLS.
ETHIF- Ethernet Interface, provides independent interfaces from HW along with the VLAN manipulation.
ETHSM- Ethernet State Manager, can activate or deactivate Ethernet drivers and tranceivers to turn on/off.
ETH- Ethernet driver, is the microcontroller driver that encapsulates all the Ethernet drivers of the same type.
ETHTRCV- Ethernet Transceiver is the microcontroller driver that encapsulates all the Ethernet transceivers of the same type.
ETHSWT- Ethernet Switch Driver configures the Ethernet physical switches and encapsulates all the Ethernet switches of the same type.
SD- Service Discovery (available since Autosar 4.1.1) manages the service state when configuring the communication patterns with the abilities to routing deactivation when a service is not available. SD announces the service availability and location to other ECUs allowing CPU load reduction due to the inhibition of invalid signals. SD permits the Some/IP data transmission to multi ECUs at the same time instead on transmit this data one by one. SD interacts with the SWCs and RTE indirectly by BswM (Basic SW Manager) regarding system services since BswM changes the PDU abstraction that comes from SD and it converts them to system signals.
SOMEIPXF- Some/IP Transformer, serialization, and deserialization of data along with the LDCOM use.
UDPNM – UDP Network Manager coordinates the configuration shut-down of ECUs with Ethernet.
LDCOM- Large Data COM enables the efficient communication of large data. LDCOM does not contain some features from COM such as timeout supervision, Rx/Tx filtering, Signal invalidation, and so on; instead of these features, LDCOM is focused on SOME/IP large data transmission.
<Figure 9>
SoAd Structure
The internal structure of SoAd is the Tx/Rx stream and the socket grouping. When an upper layer sends a PDU, the Tx stream is used (or PDUR) to transmit to a socket. The PDU router can be subdivided dependently on the required configuration. Each PDU Router can have a different PDU Route destination that would connect by configuration to a specific socket. Each socket is mapped to a group of socket connections, and each socket connection group shares the same configurations that intervene in the data transmission behavior.
The socket stream transfers information of one socket to upper layer modules that would use specific interfaces in SoAd to the transmitted PDUs.
Each PDU/Socket that is being transmitted in the PDU/Socket router contains specific configurations, for example, the PDU Header ID or the UpperLayerType (being TP or Intf). The PDU from PDUR contains additional parameters such as TriggerMode or TriggerTimeout.
The socket connection group contains public IP/Ports that can be configurable, the PDU/sockets that have private IP/Port that can be configurable too; these IP/Ports can be activated or deactivated.
<Figure 10>
TCP/IP configuration, and structure
ETH module contains different PHY and each PHY contain Tx/Rx buffers with different lengths.
ETHIF contains the drivers that are specified for SW special use cases, these drivers can control different VLANs using only one PHY; therefore usually there are more VLAN controllers than Phys.
TCPIP maps the IP local address with the TCPIP controllers and they are assigned based on the configuration and protocol sets that are appointed. Each IP instance contains caches and configurations for either ARP or NDP. The TCPIP controllers are connected 1 on 1 with the TCPIP local addresses of higher abstraction. TCPIP contain the resources to transmit through sockets, for example, each socket contains its own Tx/Rx buffers, and UDP sockets use the ETH buffers directly. The TCPIP sockets can have an assigned owner by configuration (CDD, SoAd, and DHCP for example).
Automotive Ethernet use cases
DoIP
DoIP (Diagnostic over IP) is an Autosar module that is located in the service abstraction layer, DoIP is above SoAd and permits the connection of SoAd with PDUR to communicate the complete stack, as well as, to transmit to RTE directly; SoAd provides to DoIP the transmission of data using sockets, PDU data reception, and connection notification and establishment callbacks and interfaces.
DoIP permits the diagnosis of ECU (or fast reflashing) through TCP/IP protocols. Some use cases of DoIP are the ECU diagnosis through scan tool, execution of the bootloader, or reflashing the ECU; all can be done in manufacturing processes (EOL) or in the repair shop. Connected to DoIP, the ECU can behave as a gateway to communicate with a tester with the whole ECU network, or being connected to the tester on a 1 on 1 setup. Since Autosar 4.1.1, DoIP is separated from SoAd making both configurations to be simpler.
One of the biggest advantages of DoIP compared with traditional buses diagnostics (CAN, LIN FD, etc) is the fast access to private connections with different actors (VLAN networks). Due to the different protocols types that DoIP can interact with, DoIP attach a wrapper at the end of the ETH frame delivery; for example, if one UDP set is being transmitted, DoIP wraps this UDP set to introduce the set to the TCP/IP stack.
DoIP uses UDP packages to support status and configuration monitoring, whereas DoIP uses TCP packages to send diagnostic or lifetime messages taking advantage of the TCP duplex communication.
DoIP uses DCM (Diagnostic Communication Manager) to provide the VIN(Vehicle ID Number) and uses DET when DoIP found incorrect values when certain configurations are enabled.
<Figure 11>
Some/IP and SD
<Figure 4>
Some/IP and SD are BSW modules that permit the service communication among ECUs using the Ethernet network. The service communication enables the method and attributes transmission to process software in other ECUs. Some/IP is a middleware to obtain/send services, and SD discovers and handles the available service transmission. When SD discover a service to transmit, together with Some/IP, it can use UDP packages to fast broadcasting and TCP to safer unicast transmissions.
The relationship between SD, Some/IP, and SoAd is a multilevel dependency, while SD is in charge of offering available services, SoAd performs the routing/forwarding of the data packages, and Some/IP serialize and deserialize the TCP/IP message payloads by the following format {Methods, Events, Fields} that SoAd processes. This dependency requires a network-level database of ECUs to configure the service methods and events that are required to know which attributes can be discovered after the de/serialization of payloads.
Some/IP-SD Header
The header of Some/IP is:
ID message to identify the Service ID (which service is used) and the Method ID (which method is used).
Length is the payload size + Cliend ID size + Session ID size.
Request ID (Client ID and Session-ID)
Protocol version. Some/IP version to be specific.
Interface version
Message Type
Return Code
Some/IP PDU contains a header and payload. Part of the Some/IP header is mapped to the generic PDU header during the transmission among BSW modules. This Some/IP header is the service ID, method ID, and the length; the Some/IP payload might contain methods (along with their arguments), event data, or fields (attributes).
The header of Some/IP-SD contains the Some/IP header but with additional SD fields such as:
Internal Flags.
Input array length
Input array – containing meta-information and service properties.
Length of the options array.
Option array – containing meta-information and properties for each input.
Some/IP PDUs transmissions
The Some/IP messages types can be:
Request/Response Methods
Fire and Forget Methods
Event Notification
Field Getter/Setter or Notifications
<Figure one from new book>
During the PDUs transmission from SoAd to Some/IP, SoAd receives an Ethernet frame, remove (if is going to the Socket Router) or add (if it is transmitting from the Socket Router) the part {serviceID, methodID, length} in the PDU header, and package the frame into a Autosar format PDU to be forwarded to PDUR and LDCOM to later arrive to Some/IP. Some/IP in RTE performs the calling of the specific SWC function through the PDU deserialization. This deserialization of the payload contains the method information and/or called attribute.
<figure 2>
Communication Patterns
When the communication between the server (service providers) and the client (service requester) starts, the server has one OfferService reply indicating that the client can use that service. If the client requires the service usage, then the method is called, and if necessary, a result value would be returned. During the operation, the client can request the FindService. If the service is available for the client, then the server would reply with an OfferService; in another way, no response is sent. An OfferService can be configured with a timeout and usually, an OfferService returns the address where the method is located (UDP) or the port connection address (TCP).
The client has the possibility to subscribe to an event group. When the client performs a valid request of SubscribeEventGroup, the server returns an acknowledge and notify the client when any event of the group passed. The client can terminate the subscription of a event group (StopEventGroup). One subscription is replied with the address where the events shall be sent (both for TCP or UDP); while the Subscribe Acknowledge responds with the address of multicast listening. The multicast subscription of a specific event group happens only when various clients are subscribed to the same event group; the number of subscribers can be configured in the SD module.
<figure 3>
Time Sensitive Network
TSN (Time Sensitive Network) is a module that manages the time information through the master timer. In the automotive industry, TSN is not that necessary and Autosar supports only STBM (Synchronized Time-Base Manager) located in the service layer to transmit (or receive) the time through Ethernet or any vehicle network protocol. Each vehicle network protocol has its own way to handle time (communication delays, master timer setting, HW/SW timer use). The STBM has few BSW features and it is more dependent on the ECU network.
Reference
https://www.autosar.org/

