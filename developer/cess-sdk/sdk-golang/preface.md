If you need to access the CESS chain, conduct transactions, upload and download files, etc., you can install Go SDK first. This article provides multiple installation methods for Go SDK.

## SDK source code and API documentation
Please visit [GitHub](https://github.com/CESSProject/cess-go-sdk) to obtain the Go SDK source code. For more information, see [the Go SDK API documentation](https://pkg.go.dev/github.com/CESSProject/cess-go-sdk).

## Sample Program
Go SDK provides a wealth of sample programs for your reference or direct use. Examples include the following:
| Sample Files | Sample Content |
| ------------ | -------------- |
| [NewChainClient](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/chain.go#L97) | [Initialize Client](initialization.md) |
| [StoreFile](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L52) | [Store a file](object_file/StoreFile.md) |
| [StoreObject](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L167) | [Store an object](object_file/StoreObject.md) |
| [RetrieveFile](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L232) | [Retrieve a file](object_file/RetrieveFile.md) |
| [RetrieveObject](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L313) | [Retrieve an object](object_file/RetrieveObject.md) |
| [StoreFileToMiners](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L313) | [Store a file to miners](object_file/StoreFileToMiners) |
| [RetrieveFileFromMiners](https://github.com/CESSProject/cess-go-sdk/blob/main/core/process/file.go#L313) | [Retrieve a file from miners](object_file/RetrieveFileFromMiners) |