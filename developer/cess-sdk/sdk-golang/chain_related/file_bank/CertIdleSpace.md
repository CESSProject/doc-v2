This is the interface for the storage miner to authenticate the idle space to the chain, when the storage miner finishes generating idle data, it needs to call this interface to authenticate to the chain, if the authentication passes, the idle space of the storage miner will be increased.

```golang
// CertIdleSpace authenticates idle file to the chain
//   - spaceProofInfo: space proof info
//   - teeSignWithAcc: tee sign with account
//   - teeSign: tee sign
//   - teePuk: tee work public key
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - for storage miner use only
func (c *ChainClient) CertIdleSpace(spaceProofInfo SpaceProofInfo, teeSignWithAcc, teeSign types.Bytes, teePuk WorkerPublicKey) (string, error)
```
For the type definition, please refer to [SpaceProofInfo](../chain_type.md#SpaceProofInfo), [WorkerPublicKey](../chain_type.md#Type-definition)

For example code, please refer to [attestation_idle.go](https://github.com/CESSProject/cess-miner/blob/main/node/attestation_idle.go)