# Call `generate_restoral_order`

Create a transaction to restore the order. When a miner's file cannot be restored, the data segment can be declared to create a recovery order. Wait for other miners to claim the order for recovery.

```rust
pub async fn generate_restoral_order(
    &self,
    file_hash: &str,
    restoral_fragment: &str,
) -> Result<(TxHash, GenerateRestoralOrder), Box<dyn std::error::Error>>
```