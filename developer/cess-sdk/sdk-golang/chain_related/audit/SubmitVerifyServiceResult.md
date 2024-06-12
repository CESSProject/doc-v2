SubmitVerifyServiceResult is an interface used by storage miners to submit validation result of service data proof to the chain.

```golang
// SubmitVerifyServiceResult submit validation result of service data proof to the chain
//   - result: validation result of idle data proof
//   - sig: signature from tee
//   - bloomFilter: bloom filter value
//   - teePuk: tee's work public key
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) SubmitVerifyServiceResult(result types.Bool, sign types.Bytes, bloomFilter BloomFilter, teePuk WorkerPublicKey)
```

For example code, please refer to [challenge_service.go](https://github.com/CESSProject/cess-miner/blob/main/node/challenge_service.go)
