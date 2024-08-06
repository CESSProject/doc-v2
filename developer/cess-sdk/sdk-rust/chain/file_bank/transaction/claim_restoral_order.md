# Call `claim_restoral_order`

Claim the transaction that restored the order. A recovery order can only be claimed by one miner. During the recovery period, other miners will no longer be able to claim the order.

```rust
pub async fn claim_restoral_order(
    &self,
    restoral_fragment: &str,
) -> Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>> 
```