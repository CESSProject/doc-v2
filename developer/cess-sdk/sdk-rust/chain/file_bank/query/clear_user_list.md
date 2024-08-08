# Query `clear_user_list`

This auxiliary storage divides tasks into multiple blocks to prevent excessive workload in any single block. It keeps track of which user's files should be cleaned next.

```rust
/// Returns the list of account and associated territory from which the files needs to be removed.
///
/// This function divides tasks into multiple blocks to prevent excessive workload in any single block.
/// It identifies and processes the files that should be cleaned for each user.
///
/// # Arguments
///
/// * `block_hash` - An optional parameter representing the hash of the block. If provided, the function
///   will clear the user list associated with this specific block.
///
/// # Returns
///
/// * `Result<Option<BoundedVec<(AccountId32, BoundedVec<u8>)>>, Box<dyn std::error::Error>>` -
///   If successful, returns an `Option` containing a bounded vector of tuples. Each tuple consists of
///   an `AccountId32` and a bounded vector of `u8` representing user data. If there is no data to clear,
///   returns `None`. In case of an error, returns an error wrapped in a `Box`.
///
pub async fn clear_user_list(
    block_hash: Option<H256>,
) -> Result<Option<BoundedVec<(AccountId32, BoundedVec<u8>)>>, Box<dyn std::error::Error>>
```