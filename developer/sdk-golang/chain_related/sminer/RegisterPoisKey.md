This is the interface for storage miners to register pois keys and is in the second stage of storage miner registration, the first stage being the call to the [RegnstkSminer](RegnstkSminer.md) interface.

```golang
// RegisterPoisKey register pois key, storage miner registration
// requires two stages, this is the second one.
//
//   - poisKey: pois key
//   - teeSignWithAcc: tee's sign with account
//   - teeSign: tee's sign
//   - teePuk: tee's work public key
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - storage miners must complete the first stage to register for the second stage
func (c *ChainClient) RegisterPoisKey(poisKey PoISKeyInfo, teeSignWithAcc, teeSign types.Bytes, teePuk WorkerPublicKey) (string, error)
```

For the type definition, please refer to [PoISKeyInfo](../chain_type.md#PoISKeyInfo), [WorkerPublicKey](../chain_type.md#Typedefinition)

For example code, please refer to [run.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/run.go)