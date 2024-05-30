This is the interface for retrieving files from the gateway.

```golang
// RetrieveFile downloads files from the gateway
//   - url: gateway url
//   - fid: fid
//   - mnemonic: polkadot account mnemonic
//   - savepath: file save path
//
// Return:
//   - string: fid
//   - error: error message
func RetrieveFile(url, fid, mnemonic, savepath string) error
```

Example code:
```golang
package main

import (
	"bytes"
	"context"
	"fmt"
	"io"
	"log"
	"time"

	sdkgo "github.com/CESSProject/cess-go-sdk"
	"github.com/CESSProject/cess-go-sdk/core/process"
	"github.com/CESSProject/cess-go-sdk/utils"
)

// Substrate well-known mnemonic:
//
//	https://github.com/substrate-developer-hub/substrate-developer-hub.github.io/issues/613
//  - cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB
var MY_MNEMONIC = "bottom drive obey lake curtain smoke basket hold race lonely fit walk"

const PublicGateway = "http://deoss-pub-gateway.cess.cloud/"
const PublicGatewayAccount = "cXhwBytXqrZLr1qM5NHJhCzEMckSTzNKw17ci2aHft6ETSQm9"
const RetrieveFid = "Your Fid"
const SavePath = "Your file path"

func main() {
	// download file from gateway
	fid, err := process.RetrieveFile(PublicGateway, RetrieveFid, MY_MNEMONIC, SavePath)
	if err != nil {
		panic(err)
	}
}
```