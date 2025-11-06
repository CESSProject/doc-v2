This is the interface for storage miner registration, it is only used if your staking account is different from your signature account. This interface and the [RegnstkSminer](RegnstkSminer.md) interface are in the same first stage of miner registration, but you can only call one of them, there are two stages of miner registration, the second stage is to call the [RegisterPoisKey](RegisterPoisKey.md) interface.

```golang
// RegnstkAssignStaking is registered as a storage miner, unlike RegnstkSminer,
// needs to be actively staking by the staking account, which is the first stage
// of storage miner registration.
//
//   - earnings: earnings account
//   - endpoint: communications endpoint
//   - stakingAcc: staking account
//   - tibCount: the size of declaration space, in TiB
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) RegnstkAssignStaking(earnings string, endpoint []byte, stakingAcc string, tibCount uint32) (string, error)
```

For example code, please refer to [run.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/run.go)