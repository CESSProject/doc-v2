A storage node uses a combination of `multi-address` and `peerid` to uniquely identify itself, as shown in the following example:

```
/ip4/127.0.0.1/tcp/80/p2p/16Uiu2HAkucvAj8FaUwfMZunCnKB8dGTE4GzX91hVuRV33zLxzZq6
```

- *ip4*: the communication is based on an IPv4 address
- *127.0.0.1*: the IP address for communication
- *tcp*: the communication is using TCP Protocol
- *p2p*: it is a P2P communication
- *16Uiu2HAkucvAj8FaUwfMZunCnKB8dGTE4GzX91hVuRV33zLxzZq6*: the node ID

This unique identifier enables a peer to establish a correct connection and verifies the peer's identity and data information.

Please refer to [libp2p-Addressing](https://docs.libp2p.io/concepts/fundamentals/addressing/) to learn more on `multi-address`, and [libp2p-PeerID](https://docs.libp2p.io/concepts/fundamentals/peers/) on `peerid`.
