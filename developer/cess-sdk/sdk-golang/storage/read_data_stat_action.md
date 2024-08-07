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
	"_dnsaddr.boot-miner-testnet.cess.network",
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

    remoteAddrs := peer2.Addrs()

	for _, v := range remoteAddrs {
		remoteAddr, err := ma.NewMultiaddr(fmt.Sprintf("%s/p2p/%s", v, peer2.ID().String()))
		if err != nil {
			fmt.Println("NewMultiaddr err: ", err)
			continue
		}
		info, err := peer.AddrInfoFromP2pAddr(remoteAddr)
		if err != nil {
			fmt.Println("AddrInfoFromP2pAddr err: ", err)
			continue
		}

		err = peer1.Connect(context.TODO(), *info)
		if err != nil {
			fmt.Println("Connect err: ", err)
			continue
		}

		fragmentsize, err := peer1.ReadDataStatAction(info.ID, "fid", "fragment_hash")
		if err != nil {
			fmt.Println("ReadDataStatAction err: ", err)
			continue
		}
		fmt.Println("ok, fragment size is ", fragmentsize)
		return
	}
    fmt.Println("failed")
}
```