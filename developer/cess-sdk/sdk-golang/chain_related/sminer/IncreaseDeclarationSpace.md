This is the interface for storage miners to increase the size of declaration space. The declared space size corresponds to your staking. Once your staking does not meet the declared space size, you will enter a frozen state.

```golang
// IncreaseDeclarationSpace increases the size of space declared on the chain
//   - tibCount: the size of the declaration space increased, in TiB
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - the size of the declared space cannot be reduced
//   - when the staking does not meet the declared space size, you will be frozen
func (c *ChainClient) IncreaseDeclarationSpace(tibCount uint32) (string, error)
```

For example code, please refer to [increase.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/increase.go)