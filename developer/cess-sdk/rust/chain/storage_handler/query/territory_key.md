# Query `territory_key`

Ensure unique record of territory token.

```rust
/// Retrieve Territory Key
///
/// This function retrieves a unique record of a territory token. It ensures that the territory token is unique and fetches the associated information.
///
/// # Parameters
///
/// - `token`: The territory token used to identify the unique territory.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<(String, String)>`:
/// - `Some((String, String))`: A tuple containing unique information associated with the specified territory token.
/// - `None`: If no information is available for the specified territory token.
///
pub async fn territory_key(
    token: &str,
    block_hash: Option<H256>,
) -> Result<Option<(String, String)>, Box<dyn std::error::Error>> 
```