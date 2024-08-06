# Call `create_bucket`

The `create_bucket` creates the bucket. The files uploaded by the user must be located in a certain bucket, and the bucket name can only consist of numbers, letters, and special characters (_ - .).

```rust
/// Creates a new bucket with the specified name under the given account.
///
/// # Parameters
///
/// - `account`: The identifier of the account under which the bucket will be created.
/// - `bucket_name`: The name of the bucket to be created. Valid bucket name can be combination of numbers, letters, and special characters (_ - .).
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the creation of the bucket.
/// - `CreateBucket`: A structure representing the created bucket.
///
pub async fn create_bucket(
    &self,
    account: &str,
    bucket_name: &str,
) -> Result<(TxHash, CreateBucket), Box<dyn std::error::Error>>
```