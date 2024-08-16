# Query `user_hold_file_list`

The `user_hold_file_list` is used to query all files uploaded by the user.


```rust
/// Asynchronously retrieves a list of files held by a given user account.
///
/// # Parameters
/// - `account`: A valid account identifier string for which the list of files needs to be queried.
/// - `block_hash`: An optional `H256` hash representing the block hash to verify the files against.
///
/// # Returns
/// A `Result` which is:
/// - `Ok(Some(BoundedVec<UserFileSliceInfo>))` if the files are found and successfully retrieved.
/// - `Ok(None)` if no files are found for the given account.
/// - `Err(Box<dyn std::error::Error>)` if there is an error during retrieval.
///
pub async fn user_hold_file_list(
    account: &str,
    block_hash: Option<H256>,
) 
```