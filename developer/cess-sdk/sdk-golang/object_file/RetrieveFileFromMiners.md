This is the interface for retrieving the file from miners.

```golang
// RetrieveFileFromMiners Retrieve a storaged file from storage miners
//
// Receive parameter:
//   - rpcs: rpc address list
//   - fid: [fid] unique identifier for the file
//   - cipher: decryption password, if any
//   - savedir: file save directory, final save location: <savedir>/<fid>
//
// Return parameter:
//   - error: error message
//
// Preconditions:
//  1. the file to be downloaded needs to have been stored in the miner
func RetrieveFileFromMiners(rpcs []string, mnemonic, fid, cipher, savedir string) error
```

Example code:
```golang
package main

import (
	"fmt"

	"github.com/CESSProject/cess-go-sdk/core/process"
)

// Substrate well-known mnemonic:
//
//	https://github.com/substrate-developer-hub/substrate-developer-hub.github.io/issues/613
var MY_MNEMONIC = "bottom drive obey lake curtain smoke basket hold race lonely fit walk"

var RPC_ADDRS = []string{
	//testnet
	"wss://testnet-rpc.cess.network/ws/",
}

const (
	FID    = ""
	CIPHER = ""
	DIR    = "."
)

func main() {
	err := process.RetrieveFileFromMiners(RPC_ADDRS, MY_MNEMONIC, FID, CIPHER, DIR)
	fmt.Println("err: ", err)
}
```