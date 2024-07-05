This section mainly introduces how to obtain some properties of the SDK client.

```golang
package main

import (
    "context"
    "fmt"
    "time"

    cess "github.com/CESSProject/cess-go-sdk"
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
    sdk, err := cess.New(
        context.Background(),
        cess.ConnectRpcAddrs(RPC_ADDRS),
        cess.Mnemonic(MY_MNEMONIC),
        cess.TransactionTimeout(time.Second*10),
    )
    if err != nil {
        panic(err)
    }
    defer sdk.Close()

    // get sdk name
    fmt.Println(sdk.GetSDKName())

    // get the current rpc address being used
    fmt.Println(sdk.GetCurrentRpcAddr())

    // get the rpc connection status flag
    //   - true: connection is normal
    //   - false: connection failed
    fmt.Println(sdk.GetRpcState())

    // get your current account address
    //   - make sure you fill in mnemonic when you create the sdk client
    fmt.Println(sdk.GetSignatureAcc())

    // get your current account public key
    //   - make sure you fill in mnemonic when you create the sdk client
    fmt.Println(sdk.GetSignatureAccPulickey())

    // get substrate api
    fmt.Println(sdk.GetSubstrateAPI())

    // get the mnemonic for your current account
    fmt.Println(sdk.GetURI())

    // get token symbol
    fmt.Println(sdk.GetTokenSymbol())

    // get network environment
    fmt.Println(sdk.GetNetworkEnv())
}
```

Output example:
```bash
cess-sdk-go
wss://testnet-rpc0.cess.cloud/ws/
true
cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB
[70 235 221 239 140 217 187 22 125 195 8 120 215 17 59 126 22 142 111 6 70 190 255 215 125 105 211 155 173 118 180 122]
&{0xc000290200 0xc00028c090}
bottom drive obey lake curtain smoke basket hold race lonely fit walk
TCESS
cess-testnet
```