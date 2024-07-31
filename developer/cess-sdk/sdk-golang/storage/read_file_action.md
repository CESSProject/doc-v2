This is the method of downloading files from the storage network, which is done in fragments. Fortunately, you only need to download a third of the number of fragments to get the corresponding segment, and after downloading all the segments, you splice them in order to get the complete original file.

Example code:
```golang
package main

import (
	"context"
	"fmt"

	p2pgo "github.com/CESSProject/p2p-go"
	"github.com/CESSProject/p2p-go/core"
	"github.com/libp2p/go-libp2p/core/peer"
	ma "github.com/multiformats/go-multiaddr"
)

const P2P_PORT1 = 4001
const P2P_PORT2 = 4002

var P2P_BOOT_ADDRS = []string{
	//testnet
	"_dnsaddr.boot-miner-devnet.cess.network",
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

	err = peer1.ReadFileAction(target_addr.ID, "test_fid", "fragment_hash", "test_file", core.FragmentSize)
	fmt.Println("err: ", err)
}
```