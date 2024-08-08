# Call `restoral_order_complete`

Restore the order to complete the transaction. When the miner completes the storage and calculation of tags for this fragment, it calls the transaction to report. The chain will update its state to point this data to that miner.

```rust
/// Completes the restoration order transaction. This is used when a miner finishes storing and calculating tags for a data fragment,
/// and needs to report the completion to update the chain's state.
///
/// # Parameters
///
/// - `fragment_hash`: The unique hash of the data fragment that has been restored and processed.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the completion of the restoration order.
/// - `RecoveryCompleted`: A structure representing the result of the restoration order completion.
///
pub async fn restoral_order_complete(
    &self,
    fragment_hash: &str,
) -> Result<(TxHash, RecoveryCompleted), Box<dyn std::error::Error>>
```