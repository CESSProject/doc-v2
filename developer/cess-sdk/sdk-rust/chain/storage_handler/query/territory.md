# Query `territory`

This function retrieves detailed information about a specific territory owned by a account.

```rust
/// Retrieve Territory Information
///
/// This function retrieves detailed information about a specific territory owned by a account.
/// It allows querying the territory information by providing the account and the user-defined territory name.
///
/// # Parameters
///
/// - `account`: The user address used to identify the owner of the territory.
/// - `territory_name`: The user-defined name of the territory.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<TerritoryInfo>`:
/// - `Some(TerritoryInfo)`: The detailed information about the specified territory.
/// - `None`: If no information is available for the specified user address and territory name.
///
pub async fn territory(
    account: &str,
    territory_name: &str,
    block_hash: Option<H256>,
) -> Result<Option<TerritoryInfo>, Box<dyn std::error::Error>>
```