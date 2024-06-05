This section describes how to get some attributes of a p2p node.

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

	// get peer id
	fmt.Println(peer_node.ID())

	// get peer addrs
	fmt.Println(peer_node.Addrs())

	// get peer workspace
	fmt.Println(peer_node.Workspace())

	// get peer public key
	fmt.Println(peer_node.GetPeerPublickey())

	// get peer protocol prefix
	fmt.Println(peer_node.GetProtocolPrefix())

	// get peer bootnode
	fmt.Println(peer_node.GetBootnode())

	// get peer private key file
	fmt.Println(peer_node.PrivatekeyPath())

	// get peer host
	fmt.Println(peer_node.GetHost())

	// get peer DHT table
	fmt.Println(peer_node.GetDHTable())

	// get peer recv flag
	fmt.Println(peer_node.GetRecvFlag())

	// get peer dirs
	fmt.Println(peer_node.GetDirs())
}
```