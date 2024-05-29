This is the interface for logging off the gateway.

```golang
// DestroyOss destroys the oss role of the current account
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) DestroyOss() (string, error)
```

For example code, please refer to [exit.go](https://github.com/CESSProject/DeOSS/blob/main/cmd/cmd/exit.go)