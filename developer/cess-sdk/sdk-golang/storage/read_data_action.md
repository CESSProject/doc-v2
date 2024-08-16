This is a method to download data from the storage network. It returns the size of the data and an error. If you are not sure whether the other party has the file you need, you can query it through the [ReadDataStatAction](read_data_stat_action.md) method first.

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

var save_file = "save_file"

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

	// you need to put the test.txt file in ./peer2/file directory
	// note: the size of test.txt should not be less than 8388608
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	size, err := peer1.ReadDataAction(ctx, peer2.ID(), "test.txt", save_file)
	if err != nil {
		fmt.Println("ReadDataAction err: ", err)
		return
	}
	fmt.Println("success, size: ", size)
}
```