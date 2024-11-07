# Query `total_power`

The total idle space provided by the storage nodes in the entire network.

```rust
/// Retrieve Total Idle Space Power
///
/// This function retrieves the total amount of idle space power provided by storage nodes across the entire network.
/// It reflects the total capacity available from all nodes for storing data, which may be utilized for various tasks.
///
/// # Parameters
///
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<u128>`:
/// - `Some(u128)`: The total amount of idle space power provided by the storage nodes, in bytes.
/// - `None`: If no information is available for the specified block hash.
///
pub async fn total_power(
    block_hash: Option<H256>,
) -> Result<Option<u128>, Box<dyn std::error::Error>> 
```