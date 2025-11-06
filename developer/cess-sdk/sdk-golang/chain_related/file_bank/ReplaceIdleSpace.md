The interface for replacing idle files with inservice files. 
It is called by storage nodes.

```golang
// ReplaceIdleSpace replaces idle files with inservice files
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
//   - for storage node only
func (c *ChainClient) ReplaceIdleSpace(spaceProofInfo SpaceProofInfo, teeSignWithAcc, teeSign types.Bytes, teePuk WorkerPublicKey) (string, error)
```
For the type definition, please refer to [SpaceProofInfo](../chain_type.md#SpaceProofInfo), [WorkerPublicKey](../chain_type.md#Type-definition)

For example code, please refer to [replace_idle.go](https://github.com/CESSProject/cess-miner/blob/main/node/replace_idle.go)
