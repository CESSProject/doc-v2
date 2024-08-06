# Call `claim_restoral_noexist_order`

Claim a transaction for a restoration order that does not currently exist. The prerequisite for claiming this type of order is that the miner exits, and all files belonging to the miner can be claimed and restored. Then the remaining miners claim it through this transaction.

```rust
pub async fn claim_restoral_noexist_order(
    &self,
    account: &str,
    file_hash: &str,
    restoral_fragment: &str,
) -> Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>> 
```

