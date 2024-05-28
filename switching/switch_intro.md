# Switch Introduction

Switches are layer 2 devices in the TCP/IP protocol suite. They operate on the L2 traffic. Many of the switch functionalities have been implemented at the hardware itself.
This is to reduce latency and improve the performance.

Switch allows interconnection between many devices within a LAN. Multiple devices can communicate with each other via the switch. Switch have many ports, some have 9 and upto
256 on server grade switches.

To pass data between two different hosts switch can either flood the traffic over to all other ports or it can learn over time which device is on which port and so on and direct the traffic towards
that port.

## Mac address Lookup and learning

Upon switch bootup the hardware might contain no information about the connected hosts. May be so if it is not preprogrammed etc.
A packet comes from a host with Mac address M1 to deliver to a host with Mac address M2. Switch now floods the packets over all the ports and waits for the destination to respond back.
If the destination responds back, it might respond back on a particular port. Knowing this, switch will make an entry of mac M2 and its corresponding port number in a table.
Any transactions to the mac addresses M1 and M2 now wouldn't have to be a flooded frames but rather the lookup performed in the switch table to forward the frames on
particular ports. Over the time, switch table would contain all the mappings of hosts connected on its ports.

Layer 2 has both control and data plane protocols. Below are the list of control and data plane protocols with in the switch.

| S.No | Protocol Name | Area |
|------|---------------|------|
| 1 | ARP | Control Plane |
| 2 | MACsec | Data Plane |
| 3 | PTP | Control Plane |
| 4 | AVTP | Data Plane |
