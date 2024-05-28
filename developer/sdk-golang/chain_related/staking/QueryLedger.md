This is the interface to the query validator ledger.

```golang
// QueryLedger query the staking ledger
//   - accountID: account id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - StakingLedger: staking ledger
//   - error: error message
func (c *ChainClient) QueryLedger(accountID []byte, block int32) (StakingLedger, error)
```
For the type definition, please refer to [StakingLedger](../chain_type.md#StakingLedger)
Example code:
```golang
package main

import (
	"context"
	"fmt"
	"time"

	sdkgo "github.com/CESSProject/cess-go-sdk"
	"github.com/CESSProject/cess-go-sdk/utils"
)

var RPC_ADDRS = []string{
	//testnet
	"wss://testnet-rpc0.cess.cloud/ws/",
	"wss://testnet-rpc1.cess.cloud/ws/",
	"wss://testnet-rpc2.cess.cloud/ws/",
}

func main() {
	sdk, err := sdkgo.New(
		context.Background(),
		sdkgo.ConnectRpcAddrs(RPC_ADDRS),
	)
	if err != nil {
		panic(err)
	}
	defer sdk.Close()

    account_id, err := utils.ParsingPublickey("cXfyomKDABfehLkvARFE854wgDJFMbsxwAJEHezRb6mfcAi2y")
	if err != nil {
		panic(err)
	}

	fmt.Println(sdk.QueryLedger(account_id, -1))
}
```