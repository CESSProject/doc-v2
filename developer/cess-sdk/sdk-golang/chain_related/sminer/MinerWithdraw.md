This is the interface for storage miners to withdaw stakings. You must be in exied state to withdaw them.

```golang
// MinerWithdraw withdraws all staking
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - must be an exited miner to withdraw
//   - wait a day to withdraw after pre-exit
func (c *ChainClient) MinerWithdraw() (string, error)
```

For example code, please refer to [withdraw.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/withdraw.go)