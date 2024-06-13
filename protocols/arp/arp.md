# ARP protocol

ARP allows one device to discover the destination device's physical address on a LAN. This is done with a series of ARP queries followed by an ARP reply from the destionation device. If there is no destination device, there wouldn't be any reply.

```c
|<--- 2 ----->|<----- 2 ----->|<---- 1 ---->|<---- 1 ---->
| Hw type     | Protocol Type | HA Length   | PA Length   |
|<--------------------------->|<--------------------------|
| protocol operation          |  sender mac address[0,1]  |
|-----------------------------|---------------------------|
| sender mac address [2,3,4,5]| sender proto address      |
|-------------|--------------------------------------------
| cont.       |             target mac address            |
|-------------|-------------------------------------------|
| cont.       |        target proto address               |
|---------------------------------------------------------|
```

Typical ARP header looks like the following in C.

```c
struct arp_hdr {
    u16 hwtype;
    u16 protocol_type;
    u8 ha_len;
    u8 protocol_addr_len;
    u16 operation;
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

- protocol_type is 0x0800 - ipv4.
- ha_len - mac address length (6).
- protocol_addr_len - 4.
- operation - 1 for ARP request and 2 for ARP reply.
- sender_hw_addr - mac address of the sending device.
- sender_proto_addr - ip address of the sender.
- target_hw_addr - mac address of the destination device.
- target_proto_addr - ip address of the destination.

## Typical frame exchange behavior

1. Higher layer such as IP would want to send out a frame to a destination.
2. IP queries for Mac address of the destination in the ARP cache.
3. If a matching mac for a given IP is hit, it is returned to the IP. No ARP queries will be sent here.
4. If there is no match, ARP layer would initiate an ARP request frame.
    1. The target_hw_addr will be all 0s.
    2. The target_proto_addr will be the destination address. 
5. If a destination is found, the host would receive an ARP reply containing the destination mac address.
    1. Here the destination would update its ARP table if the sender's ip and mac address mapping has been done.
    2. If the ip and mac address mapping is not done, the destination would add the sender in its ARP table.
    3. The destination mac and ip addresses will now be in the sender_hw_addr and sender_proto_addr of ARP reply. 
6. ARP will update its cache, notify IP layer that a destination mac is found.
7. IP layer would then fill in the destination address in the frame and sends out the packet.

## Gratituous ARP

Gratituous ARP is initiated by the device without having an ARP request.

The device sets the operation as ARP Reply, sets its sender mac and ip addresses, sets its destination ip address as the sender ip address and the destination mac can either be all 0s and all Fs.


Every destination that host has communciated to, the host keeps the cache of the destination mac and the corresponding ipv4 address.
It also keeps a lifetime of the entry. Meaning that, ARP entries can expire since the hosts may come and go on to a network as well.
Sometimes, the host may get a different ip address or the Mac address of the host is changed (by a user / software).

A typical ARP entry would look like as follows,

```c

struct arp_entry {
    uint8_t dest_mac[6];
    uint32_t dest_ipaddr;
    uint32_t lifetime_sec;
    uint32_t arp_state;
};
```

`arp_state` notes the type of entry. Entry can be either `Resolved` or `InProgress`.

## Attacks on ARP protocol

ARP spoofing is one of most known attack that allows an attacker to spoof a device's ipaddress and act as the "real" destination by manipulating the device's ARP table. Once a spoof message has been sent, the sender would direct traffic to the spoofed destination thus letting the attacker listen / consume / utilize the traffic for other purposes. 

Generally to avoid this, there are little or no solution. Generally the receiver must assume that a spoof must happen within a specific window to detect and block the traffic. The sender could change Mac addresses everytime they boot / initialize traffic / come out of sleep. This helps avoid getting spoofed or let an attacker "learn" about hardware macaddress early on using the data before.

### Anonymizing the mac address

Anonymizing the mac address sometimes help point out an attacker. An attacker would listen to this ARP request and tries to replay this over on the network with her/ his own mac address. This enables the firewall to detect an attack node and take further actions (black list for a time duration and so on).

## Transmit path

Transmit path of ARP is initiated by 

1. A higher layer such as IPv4.
2. An ARP request from a neighboring Host.

### Higher layer query
Upon a query for the destination mac address, resulting in a failure, an ARP request is initiated by the device. ARP request may be periodic in nature because the destination may be offline / unreachable / packet loss. Once an ARP reply is heard, no more ARP requests will be seen. Sometimes Gratituous ARPs may be seen as well.


## Receive path



## Linux kernel's ARP tables

Linux kernel exposes the ARP entries in `/proc/net/arp`.
