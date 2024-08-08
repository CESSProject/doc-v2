# Query `bucket`

The `bucket` interface is used to query bucket information associated with an account.

```rust
/// Asynchronously retrieves the `BucketInfo` associated with a given account and bucket name.
///
/// # Parameters
/// - `account`: A valid account identifier string for which the bucket information needs to be queried.
/// - `bucket_name`: A string slice that holds the name of the bucket.
/// - `block_hash`: An optional `H256` hash representing the block hash to verify the bucket against.
///
/// # Returns
/// A `Result` which is:
/// - `Ok(Some(BucketInfo))` if the bucket is found and successfully retrieved.
/// - `Ok(None)` if the bucket is not found for the given account.
/// - `Err(Box<dyn std::error::Error>)` if there is an error during retrieval.
///
pub async fn bucket(
    account: &str,
    bucket_name: &str,
    block_hash: Option<H256>,
) -> Result<Option<BucketInfo>, Box<dyn std::error::Error>> 
```