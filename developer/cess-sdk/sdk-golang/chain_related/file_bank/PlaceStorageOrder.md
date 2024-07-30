This is the interface used to generate file storage orders.

```golang
// PlaceStorageOrder place an order for storage file
//   - fid: file identification
//   - file_name: file name
//   - bucket_name: bucket name
//   - territory_name: territory name
//   - segment: segment info
//   - owner: account of the file owner
//   - filesize: file size
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) PlaceStorageOrder(fid, file_name, bucket_name, territory_name string, segment []SegmentDataInfo, owner []byte, file_size uint64) (string, error)
```
For the type definition, please refer to [SegmentDataInfo](../chain_type.md#SegmentInfo)

For example code, please refer to [put_object.go](https://github.com/CESSProject/DeOSS/blob/main/node/put_object.go)