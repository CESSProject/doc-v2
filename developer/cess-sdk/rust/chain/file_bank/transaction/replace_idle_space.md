# Call `replace_idle_space`

The `replace_idle_space` submits a transactions to replace idle space. After the miner stores the service data, there will be a part of the idle space to be replaced, and the miner completes the update of the relevant data of the idle space through this transaction.

```rust
/// Submits a transaction to replace idle space. This is used after a miner stores service data and needs to update the relevant data for the idle space.
///
/// # Parameters
///
/// - `idle_sig_info`: Information about the idle space signature that needs to be updated.
/// - `tee_sig_need_verify`: Signature that needs verification from the Trusted Execution Environment (TEE).
/// - `tee_sig`: The TEE signature used for validation and integrity checks.
/// - `tee_puk`: The public key of the TEE used for verifying the signature.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the replacement of idle space.
/// - `ReplaceIdleSpace`: A structure representing the result of the idle space replacement.
///
pub async fn replace_idle_space(
    &self,
    idle_sig_info: IdleSigInfo,
    tee_sig_need_verify: TeeSigNeedVerify,
    tee_sig: TeeSig,
    tee_puk: TeePuk,
) -> Result<(TxHash, ReplaceIdleSpace), Box<dyn std::error::Error>>
```