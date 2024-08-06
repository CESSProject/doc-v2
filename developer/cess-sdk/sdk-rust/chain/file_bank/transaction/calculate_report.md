# Call `calculate_report`

The `calculate_report` is used to calculate transactions completed by tag reporting. For subsequent storage proof, the miner needs to calculate the fragment's tag. After completing the calculation, the miner needs to report to the chain. After reporting, the computing power will be increased for miners, and this part of the data will be included in the challenge.

```rust
pub async fn calculate_report(
    &self,
    tee_sig: &str,
    account: &str,
    digest: BoundedVec<DigestInfo>,
    file_hash: &str,
) -> Result<(TxHash, CalculateReport), Box<dyn std::error::Error>>
```