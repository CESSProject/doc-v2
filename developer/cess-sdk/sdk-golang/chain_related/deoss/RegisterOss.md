This is the interface for registering as a gateway.

```golang
// RegisterOss registered as oss role
//   - domain: domain name, can be empty
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) RegisterOss(domain string) (string, error)
```

For example code, please refer to [run.go](https://github.com/CESSProject/DeOSS/blob/main/cmd/cmd/run.go)