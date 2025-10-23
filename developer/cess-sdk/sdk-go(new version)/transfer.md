## Send transaction

The following is an example of using the chain.Client client to concurrently send transfer transactions to the CESS chain:

```golang
	cli, err := chain.NewLightCessClient(
		"white income exile ethics sick excess water deliver medal jump update fault",
		[]string{"wss://t2-rpc.cess.network"},
	)
	if err != nil {
		log.Fatal(err)
	}
	total, errCount := 4000, &atomic.Int32{}
	wg := sync.WaitGroup{}
	wg.Add(total)
	st := time.Now()
	pool, err := ants.NewPool(500)
	if err != nil {
		log.Fatal(err)
	}
	for i := range total {
		idx := i
		pool.Submit(func() {
			defer wg.Done()
			tx, err := cli.TransferToken("cXjTYBWUY68uGG2t3ShAhmLtNhz3WdBfXrYn4XaQYg5pKLZcF", "1000000000000000000", nil, nil) // transferred 1 $CESS 
			if err != nil {
				log.Println(err)
				errCount.Add(1)
				return
			}
			log.Println(idx, "success,block hash:", tx)
		})
	}
	wg.Wait()
	log.Println("time:", time.Since(st), "total:", total, "errors:", errCount.Load())
```

In the above example, the last two parameters of the cli.TransferToken method are `caller` (*signature.KeyringPair, user-specified caller account) and `event`(any, Corresponding event pointer) respectively. When `caller` is nil, the account configured when creating the client is used by default. When `event` is nil, the event is not parsed by default. You can find the events you need in `chain/events.go` or at [go-substrate-rpc-client](https://github.com/centrifuge/go-substrate-rpc-client/tree/v4.2.1/types). The CESS chain client is built on `go-substrate-rpc-client`, so it supports the use of event types in `go-substrate-rpc-client`.

For other transactions, please refer to the above transfer transactions. The transaction methods in the CESS chain client natively support concurrency, so you can call them simultaneously in different goroutines without doing too much extra operations.