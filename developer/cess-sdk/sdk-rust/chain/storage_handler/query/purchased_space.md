# Query `purchased_space`

The space currently purchased by users across the entire network.

```rust
/// Retrieve Total Purchased Space
///
/// This function retrieves the total amount of space currently purchased by users across the entire network.
/// It allows querying the purchased space information at a specified block hash or the latest block.
///
/// # Parameters
///
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<u128>`:
/// - `Some(u128)`: The total amount of space currently purchased by users across the network, in bytes.
/// - `None`: If no purchased space information is available for the specified block hash.
///
pub async fn purchased_space(
    block_hash: Option<H256>,
) -> Result<Option<u128>, Box<dyn std::error::Error>>
```