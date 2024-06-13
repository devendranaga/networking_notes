# IPv4



```c

|<--4--->|<--4-->|<----8---->|<-----------16--------------------->|
|Version | IHL   | DSCP      |             Total Length           |
|----------------------------|---|----|----|----------------------|
| Identification             | R | DF | MF | Fragmentation offset |
|----------------|-----------|------------------------------------|
|   TTL          | Protocol  |        Header checksum             |
|-----------------------------------------------------------------|
|              source address                                     |
|-----------------------------------------------------------------|
|              destination address                                |
|-----------------------------------------------------------------|
|              options                                            |
|-----------------------------------------------------------------|

```

- Version is 4 for current IPv4 protocol.
- IHL is multiples of 4. Header length is fixed 20 bytes when no options are present and can be 60 max when options are used.
- Total length (16) is the ipv4 header length and data.
- DSCP - last two bits of DSCP are used for ECN (Explicit Congestion notification).
- Identification - used to allow destination host to determine which packet a newly arrived fragment belongs to. Fragments can be set a fragment ID and all fragments having this id are used for reassembly.
- R - reserved bit (1)
- DF - dont' fragment (1)
- MF - more fragments (1)
- fragmentation offset (13) - multiples of 8 except for the last fragment (describe offset of this fragment in a large packet) maximum of 8192 fragments per datagram.
- TTL - time to live field (8) - decremented everytime a packet is received. If it reaches 0, the packet will be discarded. TTL is used as a expiry counter for a packet.
- protocol (8) - higher layer l4 protocol that is being used.
- Header checksum - add all the contents of the header (16 bits each grouped) and take ones complement of the result.
      When a router changes any content of the ipv4 header, the header checksum must be recomputed. Example, decrement in TTL or fragmentation offset etc.
- Source address - (32)
- Destination address - (32)
- options - multiples of 4 bytes. (TLV type of fields and can be in any order).

  Some most used Ipv4 options:

| S.No | Option Name | Description |
|------|-------------|-------------|
| 1 | Security | Specifies how secret the datagram is (or more easy way to find the juicy information) |
| 2 | Strict source routing | Gives the complete path to be followed |
| 3 | Loose source routing | Gives a list of routers not to be missed |
| 4 | Record route | Makes each router append its IP address |
| 5 | Timestamp | Makes each router append its address and timestamp |

## Use of DF bit

Sometimes DF is used within the ICMP frames to discover the next hop distance or its TTL. This is used to perform Path MTU discovery. PMTU discovery is explained in further sections.

DF means that the machines the packet goes through, cannot fragment that particular packet. If the machine cannot fragment the packet, it simply responds back with the ICMP fragmentation needed response.

Some firewall disallow many ICMP frames, exceptions are echo request and echo reply. So if a fragmentation needed frame is sent by the destination, firewall before the host might've dropped it as well. It is required to configure firewall in such a way to allow the ICMP frame with fragmentation needed code.

## Use of TTL

## Subnet masks and addressing scheme




