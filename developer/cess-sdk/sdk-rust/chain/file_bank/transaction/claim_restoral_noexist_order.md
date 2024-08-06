# Call `claim_restoral_noexist_order`

The `claim_restoral_noexist_order` claims a transaction for restoration order that does not currently exist. The prerequisite for claiming this type of order is that the miner exits, and all files belonging to the miner can be claimed and restored. Then the remaining miners claim it through this transaction.

```rust
/// Asynchronously claim a transaction for restoration order that does not currently exist.
///
/// This function is used to claim a restoration order for a file fragment that does not currently exist.
/// The prerequisite for claiming this type of order is that the miner has exited, and all files belonging
/// to the miner can be claimed and restored. Remaining miners can then claim it through this transaction.
///
/// # Arguments
///
/// * `account` - A reference to a string representing the account identifier.
/// * `file_hash` - A reference to a string representing the hash of the file to be restored.
/// * `restoral_fragment` - A reference to a string representing the fragment of the file to be restored.
///
/// # Returns
///
/// * `Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>>` -
///   If successful, returns a tuple containing the transaction hash (`TxHash`) and the claim restoral order
///   details (`ClaimRestoralOrder`). In case of an error, returns an error wrapped in a `Box`.
///
pub async fn claim_restoral_noexist_order(
    &self,
    account: &str,
    file_hash: &str,
    restoral_fragment: &str,
) -> Result<(TxHash, ClaimRestoralOrder), Box<dyn std::error::Error>> 
```

