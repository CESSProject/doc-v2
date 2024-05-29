SubmitVerifyIdleResult is an interface used by storage miners to submit validation result of idle data proof to the chain.

```golang
// SubmitVerifyIdleResult submit validation result of idle data proof to the chain
//   - totalProofHash: total idle data proof hash value
//   - front: idle data pre-offset
//   - rear: back offset of idle data
//   - accumulator: accumulator value
//   - result: validation result of idle data proof
//   - sig: signature from tee
//   - teePuk: tee's work public key
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) SubmitVerifyIdleResult(totalProofHash []types.U8, front, rear types.U64, accumulator Accumulator, result types.Bool, sig types.Bytes, teePuk WorkerPublicKey) (string, error) 
```

For example code, please refer to [challenge_idle.go](https://github.com/CESSProject/cess-miner/blob/main/node/challenge_idle.go)
