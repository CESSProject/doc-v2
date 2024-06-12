This is the method to read the fragment size from other peer nodes, when you are not sure if the other party has a certain fragment, you can use this method to get its size first.

Example code:
```golang
package main

import (
	"context"
	"fmt"
	"log"

	p2pgo "github.com/CESSProject/p2p-go"
	"github.com/libp2p/go-libp2p/core/peer"
	ma "github.com/multiformats/go-multiaddr"
)

const P2P_PORT1 = 4001
const P2P_PORT2 = 4002

var P2P_BOOT_ADDRS = []string{
	//testnet
	"_dnsaddr.boot-miner-devnet.cess.cloud",
}

func main() {
	ctx := context.Background()

	// peer1
	peer1, err := p2pgo.New(
		ctx,
		p2pgo.Workspace("./peer1"),
		p2pgo.ListenPort(P2P_PORT1),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer1.Close()

	// peer2
	peer2, err := p2pgo.New(
		ctx,
		p2pgo.Workspace("./peer2"),
		p2pgo.ListenPort(P2P_PORT2),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer2.Close()

	maddr, err := ma.NewMultiaddr(fmt.Sprintf("/ip4/127.0.0.1/tcp/%d/p2p/%s", P2P_PORT2, peer2.ID()))
	if err != nil {
		panic(err)
	}

	target_addr, err := peer.AddrInfoFromP2pAddr(maddr)
	if err != nil {
		panic(err)
	}

	err = peer1.Connect(ctx, *target_addr)
	if err != nil {
		panic(err)
	}

	fragmentsize, err := peer1.ReadDataStatAction(target_addr.ID, "test_fid", "fragment_hash")
	log.Println("fragment size: ", fragmentsize, " err: ", err)
}
```