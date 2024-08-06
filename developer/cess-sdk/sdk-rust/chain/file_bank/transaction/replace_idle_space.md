# Call `replace_idle_space`

The `replace_idle_space` submits a transactions to replace idle space. After the miner stores the service data, there will be a part of the idle space to be replaced, and the miner completes the update of the relevant data of the idle space through this transaction.

```rust
pub async fn replace_idle_space(
    &self,
    idle_sig_info: IdleSigInfo,
    tee_sig_need_verify: TeeSigNeedVerify,
    tee_sig: TeeSig,
    tee_puk: TeePuk,
) -> Result<(TxHash, ReplaceIdleSpace), Box<dyn std::error::Error>>
```