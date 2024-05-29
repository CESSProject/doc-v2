This is the interface for storage miner registration and is in the first phase of storage miner registration, which is two phases, and the second phase is the call to the [RegisterPoisKey](RegisterPoisKey.md) interface.

```golang
// RegnstkSminer registers as a storage miner,
// which is the first stage of storage miner registration.
//
//   - earnings: earnings account
//   - peerId: peer id
//   - staking: number of staking, the unit is CESS
//   - tibCount: the size of declaration space, in TiB
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) RegnstkSminer(earnings string, peerId []byte, staking uint64, tibCount uint32) (string, error)
```

For example code, please refer to [run.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/run.go)