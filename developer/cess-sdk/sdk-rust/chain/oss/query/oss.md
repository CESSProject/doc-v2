# Query `oss`

Retrieves the OSS information for a given account.

```rust
/// Retrieves the OSS information for a given account. This function allows querying the details
/// of an OSS registered by the specified account, including its endpoint and domain description.
///
/// # Parameters
///
/// - `account`: The identifier of the account whose OSS information is being queried.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<OssInfo>`:
/// - `Some(OssInfo)`: The OSS information associated with the specified account, including the peer ID and domain.
/// - `None`: If the account is not registered as an OSS or does not exist.
///
pub async fn oss(
    &self,
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<OssInfo>, Box<dyn std::error::Error>> 
```