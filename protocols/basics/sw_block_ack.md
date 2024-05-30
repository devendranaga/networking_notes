# Block ACK based protocol

To improve the throughput a single ack can be sent back for a group of packets that are sent. Such an algorithm can be sit on top of udp to improve the
packet delivery a bit more and allows a single ACK to be sent for a series of packets.

This aims to cut down reverse control traffic back letting the sender and receiver use real data for communication instead of sending acks.

One example of the block ACK based protocol is as follows.

```c
struct packet {
    uint8_t data[1432];
};

struct burst_req {
    uint32_t blocks_count;
    struct packet blocks[31];
};
```

Here the structure `burst_req` always sends out 31 frames each of 1432 bytes in size (excluding the l4 header and below).
The sender respond back with an ACK frame.

```c
struct burst_resp {
    uint32_t ack;
};
```

The `ack` is a 32 bit field of which each bit represents if a block has not been received. If the last bit 31 is set then all blocks have been successfully received.
The protocol allows to send maximum of 31 blocks each of 1432 bytes in size.

Missed blocks are included in the next set of blocks to be sent. This helps with retransmission.
