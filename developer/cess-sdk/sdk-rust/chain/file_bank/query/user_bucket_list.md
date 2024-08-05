# Query `user_bucket_list`

```rust
/// Retrieves a list of user buckets associated with the specified account.
/// 
/// # Parameters
/// 
/// - `account`: A reference to a string slice that holds the account identifier.
/// - `block_hash`: An optional parameter specifying a block hash of type `H256`. 
///   If provided, the function will retrieve the user buckets as of the specified block.
/// 
/// # Returns
/// 
/// - `Result<Option<BoundedVec<BoundedVec<u8>>>, Box<dyn std::error::Error>>`: 
///   The function returns a `Result` which on success contains an `Option`:
///   - `Some(BoundedVec<BoundedVec<u8>>)` if the user has associated buckets.
///   - `None` if the user does not have any associated buckets.
///   On failure, it returns an error wrapped in a `Box<dyn std::error::Error>`.
/// 
pub async fn user_bucket_list(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<BoundedVec<BoundedVec<u8>>>, Box<dyn std::error::Error>>
```