This is the interface for storing miner pre-exits, after calling this interface, you need to wait one day to withdraw your staking.

```golang
// MinerExitPrep pre-exit storage miner
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - after pre-exit, you need to wait for one day before it will automatically exit
//   - cannot register as a storage miner again after pre-exit
func (c *ChainClient) MinerExitPrep() (string, error)
```

For example code, please refer to [exit.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/exit.go)