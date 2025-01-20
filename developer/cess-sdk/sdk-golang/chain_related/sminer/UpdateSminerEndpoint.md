This is the interface for storage miners to update their endpoint.

```golang
// UpdateSminerEndpoint update address for storage miner
//   - endpoint: address
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) UpdateSminerEndpoint(endpoint []byte) (string, error)
```

For example code, please refer to [update.go](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/update.go)