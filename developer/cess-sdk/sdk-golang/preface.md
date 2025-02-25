If you need to access the CESS chain, conduct transactions, upload and download files, etc., you can install Go SDK first. This article provides multiple installation methods for Go SDK.

## SDK source code and API documentation
Please visit [GitHub](https://github.com/CESSProject/cess-go-sdk) to obtain the Go SDK source code. For more information, see [the Go SDK API documentation](https://pkg.go.dev/github.com/CESSProject/cess-go-sdk).

## Sample Program
Go SDK provides a wealth of sample programs for your reference or direct use. Examples include the following:
| Sample Files | Sample Content |
| ------------ | -------------- |
| [NewChainClient](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/chain.go#L99) | [Initialize Client](initialization.md) |
| [StoreFile](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/gateway.go#L49) | [Store files on the gateway](object_file/StoreFile.md) |
| [StoreObject](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/gateway.go#L157) | [Store objects on the gateway](object_file/StoreObject.md) |
| [RetrieveFile](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/gateway.go#L218) | [Retrieve files from the gateway](object_file/RetrieveFile.md) |
| [RetrieveObject](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/gateway.go#L300) | [Retrieve objects from the gateway](object_file/RetrieveObject.md) |
| [StoreFileToMiners](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/storage.go#L50) | [Store a file to miners](object_file/StoreFileToMiners) |
| [RetrieveFileFromMiners](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/storage.go#L175) | [Retrieve a file from miners](object_file/RetrieveFileFromMiners) |