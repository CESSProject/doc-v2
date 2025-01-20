This is the interface for the gateway to update its own information.

```golang
// UpdateOss update oss's peerId or domain
//   - domain: domain name
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) UpdateOss(domain string) (string, error)
```

For example code, please refer to [run.go](https://github.com/CESSProject/DeOSS/blob/main/cmd/cmd/run.go)