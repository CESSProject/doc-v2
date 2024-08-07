# Query `total_space`

The space currently used by service data stored in storage nodes across the entire network.

```rust
/// Retrieve Total Space Used by Service Data
///
/// This function retrieves the total amount of space currently used by service data stored on storage nodes across the entire network.
/// It reflects the total capacity utilized for storing service-related data across all nodes.
///
/// # Parameters
///
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<u128>`:
/// - `Some(u128)`: The total amount of space currently used by service data, in bytes.
/// - `None`: If no information is available for the specified block hash.
///
pub async fn total_space(
    block_hash: Option<H256>,
) -> Result<Option<u128>, Box<dyn std::error::Error>> 
```