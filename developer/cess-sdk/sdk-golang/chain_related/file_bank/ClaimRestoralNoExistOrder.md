This is the interface for storage miners to claim recoverable files from exited storage miners, if you want to store more files, you can find them through this interface.

```golang
// ClaimRestoralNoExistOrder claim the restoral order of an exited storage miner
//   - puk: storage miner account
//   - fid: file identification
//   - fragmentHash: fragment hash
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - for storage miner use only
func (c *ChainClient) ClaimRestoralNoExistOrder(puk []byte, fid, fragmentHash string) (string, error)
```

Example code:
```golang
package main

import (
    "context"
    "fmt"
    "time"

    sdkgo "github.com/CESSProject/cess-go-sdk"
)

// Substrate well-known mnemonic:
//
//   - https://github.com/substrate-developer-hub/substrate-developer-hub.github.io/issues/613
//   - cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB
var MY_MNEMONIC = "bottom drive obey lake curtain smoke basket hold race lonely fit walk"

var RPC_ADDRS = []string{
    //testnet
    "wss://testnet-rpc.cess.cloud/ws/",
}

func main() {
    sdk, err := sdkgo.New(
        context.Background(),
        sdkgo.ConnectRpcAddrs(RPC_ADDRS),
        sdkgo.Mnemonic(MY_MNEMONIC),
        sdkgo.TransactionTimeout(time.Second*10),
    )
    if err != nil {
        panic(err)
    }
    defer sdk.Close()

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.ClaimRestoralNoExistOrder(account_id, "fid", "fragment_hash"))
}
```