This is the interface for storage miners to increase staking.

```golang
// IncreaseCollateral increases the number of staking for storage miner
//   - accountID: storage miner account
//   - token: number of staking
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - The number of staking to be added is calculated in the smallest unit,
//     if you want to add 1CESS staking, you need to fill in "1000000000000000000"
func (c *ChainClient) IncreaseCollateral(accountID []byte, token string) (string, error)
```

For example code, please refer to [increase.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/increase.go)