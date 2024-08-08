# Query `territory_expired`

This function checks if a territory will be marked expired at a specified block number. This is normally after 100800 blocks the territory is frozen.

```rust
/// Retrieve Territory Expiration Status
///
/// This function checks if a territory will be marked expired at a specified block number.
///
/// # Parameters
///
/// - `block_number`: The block number at which to check the expiration status.
/// - `token`: The territory token used to identify the territory.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<bool>`:
/// - `Some(true)`: Indicates that the territory expiration task has been executed.
/// - `Some(false)`: Indicates that the territory expiration task has not been executed.
/// - `None`: If no information is available for the specified block number and token.
///
pub async fn territory_expired(
        block_number: u32,
        token: &str,
        block_hash: Option<H256>,
    ) -> Result<Option<bool>, Box<dyn std::error::Error>>
```
