# Call `cert_idle_space`

The `cert_idle_space` is used to authenticate transactions for idle space. The miner's certification of idle space requires the tee to challenge it. When successful, the miner can update the new space proof data on the chain.

```rust
/// Asynchronously certifies idle storage space.
///
/// This function is used to certify the idle storage space by processing the provided signature information
/// and TEE (Trusted Execution Environment) data. The certification involves verifying the provided TEE signature,
/// and upon successful certification, it returns the transaction hash and idle space certification details.
///
/// # Arguments
///
/// * `idle_sig_info` - The information related to the idle signature.
/// * `tee_sig_need_verify` - The TEE signature that needs to be verified.
/// * `tee_sig` - The TEE signature.
/// * `tee_puk` - The public key associated with the TEE signature.
///
/// # Returns
///
/// * `Result<(TxHash, IdleSpaceCert), Box<dyn std::error::Error>>` -
///   If successful, returns a tuple containing the transaction hash (`TxHash`) and the idle space certification
///   details (`IdleSpaceCert`). In case of an error, returns an error wrapped in a `Box`.
///
pub async fn cert_idle_space(
    &self,
    idle_sig_info: IdleSigInfo,
    tee_sig_need_verify: TeeSigNeedVerify,
    tee_sig: TeeSig,
    tee_puk: TeePuk,
) -> Result<(TxHash, IdleSpaceCert), Box<dyn std::error::Error>>
```