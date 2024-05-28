This is the interface for storage miners to report to the chain that a fragment's tag has been computed, and only after the tag is computed can that fragment be challenged and receive revenue.

```golang
// CalculateReport report file tag calculation completed
//   - teeSig: tee sign
//   - tagSigInfo: tag sig info
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - for storage miner use only
func (c *ChainClient) CalculateReport(teeSig types.Bytes, tagSigInfo TagSigInfo) (string, error)
```

For the type definition, please refer to [TagSigInfo](../chain_type.md#TagSigInfo)

For example code, please refer to [calculate_tag.go](https://github.com/CESSProject/cess-miner/blob/main/node/calculate_tag.go)