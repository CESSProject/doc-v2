## Installation Environment
+ Use Golang 1.23.

Please refer to [golang installation](https://go.dev/doc/install/source) to download and install the Go compilation and running environment. After Go is installed, please create a new system variable GOPATH and point it to your code directory. To learn more about GOPATH, execute the command go help gopath.

## Download Go SDK
+ [Download via Github](https://github.com/CESSProject/go-sdk)
+ [Historical version download](https://github.com/CESSProject/go-sdk/releases)

## Install Go SDK

Add the following dependencies in the go.mod file, the following takes version v0.2.1 as an example. Other versions need to be replaced with corresponding version numbers.

```bash
go get github.com/CESSProject/go-sdk@v0.2.1
```

## Creating a CESS Client

The CESS client is used to interact with the CESS chain. It can initiate various transactions, query on-chain data, call smart contracts, and monitor and analyze on-chain events.

If you just want to use it and don't want to do too much configuration, you can quickly create a lightweight client in the following way. This client has the same functions as the regular version.
```golang
	cli, err := chain.NewLightCessClient(
		"your mnemonic",
		[]string{"wss://rpc.cess.network"},
	)
    if err!=nil{
        log.Fatal(err)
    }
    log.Println(cli.Metadata)
```

You can also create a standard client by passing a series of configuration parameters.
```golang
    rpcs:=[]string{
        "wss://rpc.cess.network",
        ...
    }
    mnemonics:=[]string{
        "your mnemonic 1",
        "your mnemonic 2",
        "your mnemonic 3",
        ...
    }

	cli, err := chain.NewClient(
		chain.OptionWithRpcs(rpcs),
		chain.OptionWithAccounts(mnemonics), //Support multiple accounts
		chain.OptionWithConnNum(4), //Support establishing multiple RPC connections with the chain
        chain.OptionWithTimeout(time.Second*30),
	)
    if err!=nil{
        log.Fatal(err)
    }
    log.Println(cli.Metadata)
```

When the CESS chain client starts, a coroutine is automatically started to maintain the stability of the RPC connection with the CESS chain.Generally speaking, it is best practice to maintain 2-4 connections, which can ensure both stability and concurrent transaction performance.

When multiple accounts are configured and you do not specify an account when trading, the CESS client will evenly poll each account to initiate a transaction. You can also use the `chain.NewKeyrings` method to create a keyrings object to manually manage your account so that you can specify the account in a specific way to initiate transactions.
