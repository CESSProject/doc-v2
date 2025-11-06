This is the interface for uploading the file to miners.

```golang
// StoreFileToMiners store a file to some miners
//
// Receive parameter:
//   - file: stored file
//   - mnemonic: account mnemonic
//   - territory: territory name
//   - timeout: timeout for waiting for block transaction to complete
//   - rpcs: rpc address list
//   - wantMiner: the wallet account of the miner you want to store. if it is empty, will be randomly selected.
//
// Return parameter:
//   - string: [fid] unique identifier for the file
//   - error: error message
//
// Preconditions:
//  1. your account needs to have money, and will be automatically created if the territory you specify does not exist.
//  2. if the number of miners you specify is less than 12, file storage will be exited if even one fails.
//  3. if the number of miners you specify is greater than 11, no other miners will be found for storage.
func StoreFileToMiners(file string, mnemonic string, territory string, timeout time.Duration, rpcs []string, wantMiner []string) (string, error)
```

Example code:
```golang
package main

import (
	"fmt"
	"time"

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

const UploadFile = "file_name"
const TerritoryName = "territory_name"

var WantMiner = []string{"cX...", "cX..."}

func main() {
	fid, err := process.StoreFileToMiners(
		UploadFile,
		MY_MNEMONIC,
		TerritoryName,
		time.Second*15,
		RPC_ADDRS,
		WantMiner,
	)
	fmt.Println("fid: ", fid)
	fmt.Println("err: ", err)
}
```