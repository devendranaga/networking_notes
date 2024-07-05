# TCP protocol

TCP is a connection oriented protocol.

## Protocol description

## TCP state machine

## Handling half open connections

## Perspective of TCP statemachine from Firewall context

In the context of firewall, the firewall observes the connection between two machines (client and a server) so it needs to 
monitor both sides how the connection is progressed.

Following are the possible states in regards to the firewall context.

```c
enum class tcp_state {
  TCP_STATE_CLOSED,
  TCP_STATE_SYN_RCVD,
  TCP_STATE_SYN_ACK_SENT,
  TCP_STATE_CONN_ESTABLISHED,
  TCP_STATE_FIN_RCVD,
  TCP_STATE_FIN_ACK_SENT,
  TCP_STATE_CONN_CLOSED,  
};
```

This view point is from the server's perspective that a client has communicated.

There is a special case of RST, an RST at any point of TCP handshake and data transfer, would result in a closed state.
