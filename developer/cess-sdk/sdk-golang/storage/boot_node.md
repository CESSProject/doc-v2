The bootstrap node is used to allow storage miners to join the storage network, each storage node will be connected to a bootstrap node when it comes online, and the bootstrap node will record all the nodes it has connected to, and the address of the bootstrap node in the test network is: `_dnsaddr.boot-miner-testnet.cess.network`.

The bootstrap node publishes node information periodically through pub/sub, and other nodes can quickly discover other node information by subscribing to the connected bootstrap node. The private code is as follows:
```golang
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"strings"

	p2pgo "github.com/CESSProject/p2p-go"
	"github.com/CESSProject/p2p-go/core"
	pubsub "github.com/libp2p/go-libp2p-pubsub"
	"github.com/libp2p/go-libp2p/core/peer"
)

const P2P_PORT = 4001

var P2P_BOOT_ADDRS = []string{
	//testnet
	"_dnsaddr.boot-miner-testnet.cess.network",
}

func main() {
	ctx := context.Background()

	peer_node, err := p2pgo.New(
		ctx,
		p2pgo.Workspace("."),
		p2pgo.ListenPort(P2P_PORT),
		p2pgo.BootPeers(P2P_BOOT_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer peer_node.Close()

	// create a new PubSub service using the GossipSub router
	gossipSub, err := pubsub.NewGossipSub(ctx, peer_node.GetHost())
	if err != nil {
		panic(err)
	}

	data := strings.Split(peer_node.GetBootnode(), "/p2p/")
	room := fmt.Sprintf("%s-%s", core.NetworkRoom, data[len(data)-1])

	// join the pubsub topic called librum
	topic, err := gossipSub.Join(room)
	if err != nil {
		panic(err)
	}

	// subscribe to topic
	subscriber, err := topic.Subscribe()
	if err != nil {
		panic(err)
	}

	subscribe(subscriber, ctx, peer_node.GetHost().ID())
}

// start subsriber to topic
func subscribe(subscriber *pubsub.Subscription, ctx context.Context, hostID peer.ID) {
	var findpeer peer.AddrInfo
	for {
		msg, err := subscriber.Next(ctx)
		if err != nil {
			panic(err)
		}

		// only consider messages delivered by other peers
		if msg.ReceivedFrom == hostID {
			continue
		}

		err = json.Unmarshal(msg.Data, &findpeer)
		if err != nil {
			continue
		}

		fmt.Printf("got message: %v, from: %s\n", findpeer, msg.ReceivedFrom.String())
	}
}
```