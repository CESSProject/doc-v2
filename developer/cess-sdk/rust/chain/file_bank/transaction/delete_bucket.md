# Call `delete_bucket`

The `delete_bucket` creates transaction to delete the bucket. The prerequisite for deleting a bucket is that there are no files in the bucket before the bucket can be deleted.

```rust
/// Creates a transaction to delete the specified bucket under the given account.
///
/// # Parameters
///
/// - `account`: The identifier of the account under which the bucket exists.
/// - `bucket_name`: The name of the bucket to be deleted. The bucket must be empty before it can be deleted.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the deletion of the bucket.
/// - `DeleteBucket`: A structure representing the result of the bucket deletion.
///
pub async fn delete_bucket(
    &self,
    account: &str,
    bucket_name: &str,
) -> Result<(TxHash, DeleteBucket), Box<dyn std::error::Error>>
```