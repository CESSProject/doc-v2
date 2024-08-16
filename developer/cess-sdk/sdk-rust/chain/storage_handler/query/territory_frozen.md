# Query `territory_frozen`

This function checks if a territory will be frozen at a given block for a given territory token.

```rust
/// Retrieve Territory Freezing Status
///
/// This function checks if a territory will be frozen at a given block for a given territory token.
///
/// # Parameters
///
/// - `block_number`: The block height at which to check the freezing status.
/// - `token`: The territory token used to identify the territory that needs to be frozen.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<bool>`:
/// - `Some(true)`: Indicates that the territory freezing task has been executed.
/// - `Some(false)`: Indicates that the territory freezing task has not been executed.
/// - `None`: If no information is available for the specified block height and token.
///
pub async fn territory_frozen(
    block_number: u32,
    token: &str,
    block_hash: Option<H256>,
) -> Result<Option<bool>, Box<dyn std::error::Error>>
```