This section describes how to create a p2p node.

Example code:
```golang
package main

import (
	"context"
	"fmt"

	p2pgo "github.com/CESSProject/p2p-go"
)

const P2P_PORT = 4001

var P2P_BOOT_ADDRS = []string{
	//testnet
	"_dnsaddr.boot-miner-testnet.cess.cloud",
}

func main() {
	peer_node, err := p2pgo.New(
		context.Background(),
		p2pgo.Workspace("."),
		p2pgo.ListenPort(P2P_PORT),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer_node.Close()

	fmt.Println(peer_node.Addrs(), peer_node.ID())
}
```