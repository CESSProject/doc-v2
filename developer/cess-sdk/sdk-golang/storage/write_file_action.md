This is the method of uploading files to the storage network. This method transfers files on a fragment basis, so you must [process the files](../data_process.md) before uploading them to the storage network.

Example code:
```golang
package main

import (
	"context"
	"fmt"
	"os"

	p2pgo "github.com/CESSProject/p2p-go"
	"github.com/CESSProject/p2p-go/core"
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
    defer os.RemoveAll("./peer1")

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
    defer os.RemoveAll("./peer2")

	target_maddr, err := ma.NewMultiaddr(fmt.Sprintf("/ip4/127.0.0.1/tcp/%d/p2p/%s", P2P_PORT2, peer2.ID()))
	if err != nil {
		panic(err)
	}

	target_addr, err := peer.AddrInfoFromP2pAddr(target_maddr)
	if err != nil {
		panic(err)
	}

	err = peer1.Connect(ctx, *target_addr)
	if err != nil {
		panic(err)
	}

	err = os.WriteFile("./test_file", make([]byte, core.FragmentSize), os.ModePerm)
	if err != nil {
		panic(err)
	}
	defer os.Remove("./test_file")

	err = peer1.WriteFileAction(target_addr.ID, "test_fid", "./test_file")
	fmt.Println("err: ", err)
}
```