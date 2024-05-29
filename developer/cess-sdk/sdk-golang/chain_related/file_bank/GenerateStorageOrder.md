This is the interface used to generate file storage orders.

```golang
// GenerateStorageOrder generate a file storage order
//   - fid: file identification
//   - segment: segment info
//   - owner: account of the file owner
//   - filename: file name
//   - buckname: bucket to store the file
//   - filesize: file size
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) GenerateStorageOrder(fid string, segment []SegmentDataInfo, owner []byte, filename string, buckname string, filesize uint64) (string, error)
```
For the type definition, please refer to [SegmentDataInfo](../chain_type.md#SegmentInfo)

For example code, please refer to [putHandle.go](https://github.com/CESSProject/DeOSS/blob/main/node/putHandle.go)