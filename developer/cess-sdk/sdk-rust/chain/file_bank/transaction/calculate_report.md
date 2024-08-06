# Call `calculate_report`

The `calculate_report` is used to calculate transactions completed by tag reporting. For subsequent storage proof, the miner needs to calculate the fragment's tag. After completing the calculation, the miner needs to report to the chain. After reporting, the computing power will be increased for miners, and this part of the data will be included in the challenge.

```rust
/// Asynchronously calculates transactions completed by tag reporting.
///
/// This function is used by miners to calculate the fragment's tag for subsequent storage proof.
/// After completing the calculation, the miner needs to report to the chain. Reporting the calculation
/// results will increase the miner's computing power and include this data in the challenge.
///
/// # Arguments
///
/// * `tee_sig` - A reference to a string representing the trusted execution environment (TEE) signature.
/// * `account` - A reference to a string representing the account identifier.
/// * `digest` - A bounded vector of `DigestInfo` containing the digest information necessary for the report.
/// * `file_hash` - A reference to a string representing the hash of the file to be included in the report.
///
/// # Returns
///
/// * `Result<(TxHash, CalculateReport), Box<dyn std::error::Error>>` -
///   If successful, returns a tuple containing the transaction hash (`TxHash`) and the calculated report
///   (`CalculateReport`). In case of an error, returns an error wrapped in a `Box`.
///
pub async fn calculate_report(
    &self,
    tee_sig: &str,
    account: &str,
    digest: BoundedVec<DigestInfo>,
    file_hash: &str,
) -> Result<(TxHash, CalculateReport), Box<dyn std::error::Error>>
```