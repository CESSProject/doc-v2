# Query `unit_price`

Current unit price of storage space (Gib/days).

```rust
/// Retrieve Current Unit Price of Storage Space
///
/// This function retrieves the current unit price of storage space, measured in gibibytes per day (GiB/days), across the entire network.
/// It reflects the cost associated with storing one GiB of data for one day.
///
/// # Parameters
///
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<u128>`:
/// - `Some(u128)`: The current unit price of storage space, in GiB/days.
/// - `None`: If no information is available for the specified block hash.
pub async fn unit_price(
    block_hash: Option<H256>,
) -> Result<Option<u128>, Box<dyn std::error::Error>>
```
