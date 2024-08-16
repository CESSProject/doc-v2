This is the method to read the data size from other peer nodes. When you are not sure whether the other party has a certain data, you can use this method to obtain its size first.

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

var read_remote_file = ""

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

	fmt.Println("peer1:", peer1.Addrs(), peer1.ID())

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

	fmt.Println("peer2:", peer2.Addrs(), peer2.ID())

	peer1.Peerstore().AddAddrs(peer2.ID(), peer2.Addrs(), time.Second*5)

	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	// you need to put read_remote_file in the ./peer2/file directory
	size, err := peer1.ReadDataStatAction(ctx, peer2.ID(), read_remote_file)
	if err != nil {
		fmt.Println("ReadDataStatAction err: ", err)
		return
	}
	fmt.Println("success, remote file size: ", size)
}
```