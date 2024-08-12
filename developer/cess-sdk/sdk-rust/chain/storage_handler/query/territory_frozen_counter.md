# Query `territory_frozen_counter`

This storage is a counter set up to prevent too many freezing tasks from being executed within a block height.

```rust
/// Retrieve Territory Frozen Counter
///
/// This function retrieves the counter that tracks the number of freezing tasks executed within a specified block height.
/// This counter is set up to prevent too many freezing tasks from being executed within a single block.
///
/// # Parameters
///
/// - `block_number`: The block number at which to check the counter.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<u32>`:
/// - `Some(u32)`: The number of freezing tasks executed within the specified block height.
/// - `None`: If no counter information is available for the specified block number.
///
pub async fn territory_frozen_counter(
    block_number: u32,
    block_hash: Option<H256>,
) -> Result<Option<u32>, Box<dyn std::error::Error>>
```