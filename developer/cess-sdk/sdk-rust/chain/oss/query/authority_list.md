# Query `authority_list`

Retrieves the list of authorized operators for a given account.

```rust
/// Retrieves the list of authorized operators for a given account. This function allows querying the list of accounts
/// that have been authorized to act as operators on behalf of the specified account.
///
/// # Parameters
///
/// - `account`: The identifier of the account whose list of authorized operators is being queried.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<BoundedVec<AccountId32>>`:
/// - `Some(BoundedVec<AccountId32>)`: The list of accounts authorized as operators for the specified account.
/// - `None`: If the account has no authorized operators or does not exist.
///
pub async fn authority_list(
    &self,
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<BoundedVec<AccountId32>>, Box<dyn std::error::Error>>
```