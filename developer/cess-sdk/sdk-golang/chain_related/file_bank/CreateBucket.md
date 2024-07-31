This is the interface used to create storage buckets, all files must have corresponding storage buckets.

```golang
// CreateBucket create a bucket for owner
//   - owner: bucket owner account
//   - bucketName: bucket name
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - cannot create a bucket that already exists
//   - if you are not the owner, the owner account must be authorised to you
//
// For details on bucket naming rules, see:
//   - https://docs.cess.network/deoss/get-started/deoss-gateway/step-1-create-a-bucket#naming-conventions-for-a-bucket
func (c *ChainClient) CreateBucket(owner []byte, bucketName string) (string, error)
```

Naming conventions for a bucket:
- The name must be unique in the current account.
- Its length must be between 3 ~ 63 characters.
- Can only consist of lowercase letters, numbers, dots (.), and hyphens (-).
- Must start and end with a letter or number.
- Must not contain two adjacent periods.
- Must not be formatted as an IP address.

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
    "wss://testnet-rpc.cess.network/ws/",
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

    fmt.Println(sdk.CreateBucket(sdk.GetSignatureAccPulickey(), "my_bucket"))
}
```