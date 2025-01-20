This is the interface for storage miners to claim rewards.

```golang
// ReceiveReward to receive rewards
//
// Return:
//   - string: block hash
//   - string: earnings account for receiving payments
//   - error: error message
//
// Note:
//   - for storage miner only
//   - pass at least one idle and service challenge at the same time to get the reward
func (c *ChainClient) ReceiveReward() (string, string, error)
```

For example code, please refer to [claim.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/claim.go)