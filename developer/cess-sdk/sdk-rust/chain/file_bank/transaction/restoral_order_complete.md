# Call `restoral_order_complete`

Restore the order to complete the transaction. When the miner completes the storage and calculation of tags for this fragment, it calls the transaction to report. The chain will update its state to point this data to that miner.

```rust
pub async fn restoral_order_complete(
    &self,
    fragment_hash: &str,
) -> Result<(TxHash, RecoveryCompleted), Box<dyn std::error::Error>>
```