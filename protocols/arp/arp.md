# ARP protocol

ARP allows one device to discover the destination device's physical address on a LAN. This is done with a series of ARP queries followed by an ARP reply from the destionation device. If there is no destination device, there wouldn't be any reply.

Typical ARP header looks like the following in C.

```c
struct arp_hdr {
    u16 hwtype;
    u16 protocol_type;
    u8 ha_len;
    u8 protocol_addr_len;
    u8 sender_hw_addr[6];
    u32 sender_proto_addr;
    u8 target_hw_addr[6];
    u32 target_proto_addr;
};
```

When sender receives an ARP reply frame from the destination, it will be stored temporarily in a cache. This cache can then be used to fill the destination mac address in the ethernet frame when sending a new packet.

Also, a listening device can capture and take note of what systems are on the LAN and their addresses (IP and MAC). Generally firewalls do this to take a note of available devices on the LAN or to potentially detect malicious devices faking the ARP requests or replies.

Below are the typical settings for an ARP frame:

hwtype is mostly ethernet (1) for Ethernet based networks.
protocol_type is 0x0800 - ipv4.
ha_len - mac address length (6).
protocol_addr_len - 4.
sender_hw_addr - mac address of the sending device.
sender_proto_addr - ip address of the sender.
target_hw_addr - mac address of the destination device.
target_proto_addr - ip address of the destination.

## Typical frame exchange format

1. Higher layer such as IP would want to send out a frame to a destination.
2. IP queries for Mac address of the destination in the ARP cache.
3. If a matching mac for a given IP is hit, it is returned to the IP. No ARP queries will be sent here.
4. If there is no match is found, ARP layer would initiate an ARP request frame.
5. If a destination is found, the host would receive an ARP reply containing the destination mac address.
6. ARP will update its cache, notify IP layer that a destination mac is found.
7. IP layer would then fill in the destination address in the frame and sends out the packet.
