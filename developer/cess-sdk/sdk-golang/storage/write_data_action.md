This is how you store data to the storage network. You must first [process the file](../data_process.md) before uploading it to the storage network.

Example code:
```golang
package main

import (
	"context"
	"flag"
	"fmt"
	"time"

	p2pgo "github.com/CESSProject/p2p-go"
)

var P2P_BOOT_ADDRS = []string{
	"_dnsaddr.boot-miner-devnet.cess.cloud",
}

var send_file = ""
var send_file_hash = ""

func main() {
	ctx := context.Background()
	sourcePort1 := flag.Int("p1", 15000, "Source port number")
	sourcePort2 := flag.Int("p2", 15001, "Source port number")

	// peer1
	peer1, err := p2pgo.New(
		ctx,
		p2pgo.Workspace("./peer1"),
		p2pgo.ListenPort(*sourcePort1),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer1.Close()

	fmt.Println("node1:", peer1.Addrs(), peer1.ID())

	// peer2
	peer2, err := p2pgo.New(
		ctx,
		p2pgo.Workspace("./peer2"),
		p2pgo.ListenPort(*sourcePort2),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer2.Close()

	fmt.Println("node2:", peer2.Addrs(), peer2.ID())

	peer1.Peerstore().AddAddrs(peer2.ID(), peer2.Addrs(), time.Second*5)

	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	err = peer1.WriteDataAction(ctx, peer2.ID(), send_file, "fid", send_file_hash)
	if err != nil {
		fmt.Println("WriteDataAction err: ", err)
		return
	}
	fmt.Println("success")
}
```