In CESS distributed storage system, two methods are implemented for node discovery: one based on a built-in [node discovery mechanism](https://docs.libp2p.io/concepts/discovery-routing/overview/) of `libp2p` library and another based on the CESS blockchain network implementation. This section mainly introduces how CESS-based storage nodes discover nodes on blockchain.

As a storage node, one must have a CESS network identity - the CESS account (wallet account), and then issue a signed `register` transaction to become a storage node.

Before the storage node is activated, its identity is determined by the configuration file and the machine information. Upon registration, the node reports its identity to the blockchain network. Any node in the network can obtain identity information of other peer nodes and keep track of them to complete the node discovery process.
