# Call `delete_bucket`

The `delete_bucket` creates transaction to delete the bucket. The prerequisite for deleting a bucket is that there are no files in the bucket before the bucket can be deleted.

```rust
pub async fn delete_bucket(
    &self,
    account: &str,
    bucket_name: &str,
) -> Result<(TxHash, DeleteBucket), Box<dyn std::error::Error>>
```