# Call `generate_restoral_order`

Create a transaction to restore the order. When a miner's file cannot be restored, the data segment can be declared to create a recovery order. Wait for other miners to claim the order for recovery.

```rust
/// Creates a transaction to generate a restoration order for a file. This is used when a file cannot be restored by the current miner,
/// and a recovery order needs to be created for other miners to claim.
///
/// # Parameters
///
/// - `file_hash`: The unique hash of the file for which the restoration order is being created.
/// - `restoral_fragment`: The data segment or fragment associated with the restoration process.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the creation of the restoration order.
/// - `GenerateRestoralOrder`: A structure representing the result of the restoration order generation.
///
pub async fn generate_restoral_order(
    &self,
    file_hash: &str,
    restoral_fragment: &str,
) -> Result<(TxHash, GenerateRestoralOrder), Box<dyn std::error::Error>>
```