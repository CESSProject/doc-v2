# Query `deal_map`

The `deal_map` is used to query file storage orders.

```rust
/// Asynchronously retrieves the `DealInfo` associated with a given hash.
///
/// # Parameters
/// - `hash`: A string slice that holds the hash identifier for the deal.
/// - `block_hash`: An optional `H256` hash representing the block hash to query the deal against.
///
/// # Returns
/// A `Result` which is:
/// - `Ok(Some(DealInfo))` if the deal is found and successfully retrieved.
/// - `Ok(None)` if the deal is not found.
/// - `Err(Box<dyn std::error::Error>)` if there is an error during retrieval.
///
pub async fn deal_map(
    hash: &str,
    block_hash: Option<H256>,
) -> Result<Option<DealInfo>, Box<dyn std::error::Error>>
```
