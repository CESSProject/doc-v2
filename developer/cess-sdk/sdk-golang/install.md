## Installation Environment
+ Use Golang 1.21.

Please refer to [golang installation](https://go.dev/doc/install/source) to download and install the Go compilation and running environment. After Go is installed, please create a new system variable GOPATH and point it to your code directory. To learn more about GOPATH, execute the command go help gopath.

## Download Go SDK
+ [Download via Github](https://github.com/CESSProject/cess-go-sdk)
+ [Historical version download](https://github.com/CESSProject/cess-go-sdk/releases)

## Install Go SDK
+ go mod way

Add the following dependencies in the go.mod file, the following takes version 0.6.1 as an example. Other versions need to be replaced with corresponding version numbers.
```golang
require (
    github.com/CESSProject/cess-go-sdk v0.6.1
)
```
+ source code method
```bash
go get github.com/CESSProject/cess-go-sdk@v0.6.1
```

## Verify SDK
Run the following code to view the Go SDK version:
```golang
package main

import (
  "fmt"
  sdkgo "github.com/CESSProject/cess-go-sdk"
)

func main() {
  fmt.Println("CESS Go SDK Version: ", sdkgo.Version)
}
```