# Call `delete_file`

The `delete_file` is used to submit transaction to delete a file. A file can have multiple holders. When the file holder is 0, the meta-information of the file on the chain will be deleted. Otherwise, only the user who called this transaction will be removed from the holder list of the specified file.

```rust
pub async fn delete_file(
    &self,
    account: &str,
    file_hash: &str,
) -> Result<(TxHash, DeleteFile), Box<dyn std::error::Error>>
```