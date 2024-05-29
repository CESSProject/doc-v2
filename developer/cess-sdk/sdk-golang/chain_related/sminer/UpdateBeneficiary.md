This is the interface for storage miners to update their earnings account.

```golang
// UpdateBeneficiary updates earnings account for storage miner
//
//   - earnings: earnings account
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) UpdateBeneficiary(earnings string) (string, error)
```

For example code, please refer to [update.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/update.go)