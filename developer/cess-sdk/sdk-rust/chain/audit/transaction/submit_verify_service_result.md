# Transaction: `submit_verify_service_result`


Submits the TEE-signed verification result for a miner’s service proof to the CESS blockchain.
This ensures that miners are honestly providing service proofs and that the TEE node’s verification is authentic.

If the verification result is false, the chain increments the CountedServiceFailed value by 1.
When the count reaches 2 or more, the miner is subjected to service punishment.
If the result is true, and both idle and service verifications pass, the miner becomes eligible for a reward.

---

## Function Definition

```rust
pub async fn submit_verify_service_result(
        &self,
        service_result: bool,
        signature: BoundedVec<u8>,
        service_bloom_filter: BloomFilter,
        tee_puk: [u8; 32],
    ) -> Result<(TxHash, SubmitServiceVerifyResult), Error>
```

---

## Description

This transaction is used by a TEE node or authorized verifier to submit the verification result for a miner’s service proof.
The blockchain performs the following validation steps:

- Signature Verification – The chain verifies the TEE signature (signature) using the provided TEE public key (tee_puk) to ensure authenticity.
- Bloom Filter Comparison – The submitted service_bloom_filter is checked to confirm the correctness of the service proof.
- Failure Tracking – If the verification result is false, the miner’s CountedServiceFailed counter is incremented by 1.
- If CountedServiceFailed >= 2, the miner receives a service punishment.
- Reward Trigger – If both idle and service verifications pass, the miner is rewarded according to the CESS Reward Mechanism.

---

## Parameters

| Name                   | Type             | Description                                                                           |
| ---------------------- | ---------------- | ------------------------------------------------------------------------------------- |
| `service_result`       | `bool`           | Indicates whether the service proof verification passed (`true`) or failed (`false`). |
| `signature`            | `BoundedVec<u8>` | The TEE-signed data verifying the service proof result.                               |
| `service_bloom_filter` | `BloomFilter`    | The Bloom filter structure containing data used for proof validation.                 |
| `tee_puk`              | `[u8; 32]`       | The TEE’s public key used to verify the signature.                                    |


---

## Returns

| Type                                      | Description                                                                                                         |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `Ok((TxHash, SubmitServiceVerifyResult))` | On success, returns the transaction hash and the event emitted by the chain.                                        |
| `Err(Error)`                              | If signature verification fails, the Bloom filter check fails, or the transaction is invalid, an error is returned. |


---

## Example

```rust
use cess_rust_sdk::{
    chain::audit::transaction::StorageTransaction as AuditTx,
    polkadot::runtime_types::{bounded_collections::bounded_vec::BoundedVec, cp_bloom_filter::BloomFilter},
};


#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mnemonic = "bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice";
    let audit_tx = AuditTx::from_mnemonic(mnemonic);

    // Example parameters
    let service_result = true;
    let signature = BoundedVec(vec![0xde, 0xad, 0xbe, 0xef]);
    let service_bloom_filter = BloomFilter([0u64; 256]); // Example; replace with actual filter data
    let tee_puk: [u8; 32] = [1; 32];

    let (tx_hash, event) = audit_tx
        .submit_verify_service_result(
            service_result,
            signature,
            service_bloom_filter,
            tee_puk,
        )
        .await?;

    println!("Transaction hash: {:?}", tx_hash);
    println!("Event: {:?}", event);

    Ok(())
}

```