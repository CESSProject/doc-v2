# Query `consignment`

Stores the details of the current consignment order, using the territory token as the primary key.



```rust
/// Retrieve Consignment Details
///
/// This function retrieves the details of the current consignment order, using the territory token Id.
/// It allows querying the consignment information associated with the specified token.
///
/// # Parameters
///
/// - `token`: The territory token used to identify the consignment order.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<ConsignmentInfo>`:
/// - `Some(ConsignmentInfo)`: The consignment information associated with the specified token.
/// - `None`: If no consignment information is found for the specified token.
///
pub async fn consignment(
    token: &str,
    block_hash: Option<H256>,
) -> Result<Option<ConsignmentInfo>, Box<dyn std::error::Error>>
```