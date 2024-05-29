This is the interface used to generate file storage orders.

```golang
// UploadDeclaration generate a file storage order
//   - fid: file identification
//   - segment: segment info
//   - user: UserBrief
//   - filename: file name
//   - filesize: file size
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) UploadDeclaration(fid string, segment []SegmentList, user UserBrief, filesize uint64) (string, error)
```

For the type definition, please refer to [SegmentList](../chain_type.md#SegmentList), [UserBrief](../chain_type.md#UserBrief)

This interface has the same functionality as GenerateStorageOrder, please refer to [GenerateStorageOrder](GenerateStorageOrder.md)