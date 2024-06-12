This is the interface for storage miners to update their peer id.

```golang
// UpdateSminerPeerId update peer id for storage miner
//
//   - peerid: peer id
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) UpdateSminerPeerId(peerid PeerId) (string, error)
```

For example code, please refer to [update.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/update.go)