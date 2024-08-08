# Call `claim_restoral_order`

Claim the transaction that restored the order. A recovery order can only be claimed by one miner. During the recovery period, other miners will no longer be able to claim the order.

```rust
/// Asynchronously claims the transaction that restored the order.
///
/// This function is used to claim the transaction for restoring a file fragment. A recovery order can only be
/// claimed by one miner. During the recovery period, other miners will no longer be able to claim the order.
///
/// # Arguments
///
/// * `restoral_fragment` - A reference to a string representing the fragment of the file to be restored.
///
/// # Returns
///
/// * `Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>>` -
///   If successful, returns a tuple containing the transaction hash (`TxHash`) and the claim restoral order
///   details (`ClaimRestoralOrder`). In case of an error, returns an error wrapped in a `Box`.
///
pub async fn claim_restoral_order(
    &self,
    restoral_fragment: &str,
) -> Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>> 
```