# PMTU

Path MTU is used to discover the maximum transmission unit of a particular network device. In a network, the packets gets routed from one machine to another via various paths. Each path has a different MTU or the payload that needs to be sent is too big. So the network layer fragments the payload into small blocks and assigns them an absolute offset to join them back. Creating fragments and joining them back is a tedious topic and so PMTU is used to avoid such and make better decision about reducing packet size at the host level below MTU size.

## How does it work

In case of IPv4:

1. Router sets the MTU to a max value.
2. Router sets the DF bit in the ipv4 header and sends the frame.
3. Receiving router might reject with ICMP Fragmentation needed frame (Type 3 and code 4).
4. The sending router will readjust the MTU and sends out the frame.
5. This process will be repeated until the right MTU is found.

Once an MTU is found, the fragmentation can be avoided.

However, this also means that host must know about the maximum transmission unit before sending large payload down to the TCP/IP stack. If the host does not know the MTU and would still want to send huge payloads, the fragmentation will be inevitable.
