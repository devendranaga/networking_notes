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

