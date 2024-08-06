# Call `create_bucket`

The `create_bucket` creates the bucket. The files uploaded by the user must be located in a certain bucket, and the bucket name can only consist of numbers, letters, and special characters (_ - .).

```rust
pub async fn create_bucket(
    &self,
    account: &str,
    bucket_name: &str,
) -> Result<(TxHash, CreateBucket), Box<dyn std::error::Error>>
```