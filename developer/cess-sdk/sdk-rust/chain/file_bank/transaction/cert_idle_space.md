# Call `cert_idle_space`

The `cert_idle_space` is used to authenticate transactions for idle space. The miner's certification of idle space requires the tee to challenge it. When successful, the miner can update the new space proof data on the chain.

```rust
ub async fn cert_idle_space(
    &self,
    idle_sig_info: IdleSigInfo,
    tee_sig_need_verify: TeeSigNeedVerify,
    tee_sig: TeeSig,
    tee_puk: TeePuk,
) -> Result<(TxHash, IdleSpaceCert), Box<dyn std::error::Error>>
```