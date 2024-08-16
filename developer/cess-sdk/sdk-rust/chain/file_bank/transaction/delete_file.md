# Call `delete_file`

The `delete_file` is used to submit transaction to delete a file. A file can have multiple holders. When the file holder is 0, the meta-information of the file on the chain will be deleted. Otherwise, only the user who called this transaction will be removed from the holder list of the specified file.

```rust
/// Submits a transaction to delete a file. The file can have multiple holders.
///
/// # Parameters
///
/// - `account`: The identifier of the account initiating the deletion transaction.
/// - `file_hash`: The unique hash of the file to be deleted.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the deletion of the file.
/// - `DeleteFile`: A structure representing the result of the file deletion.
///

pub async fn delete_file(
    &self,
    account: &str,
    file_hash: &str,
) -> Result<(TxHash, DeleteFile), Box<dyn std::error::Error>>
```