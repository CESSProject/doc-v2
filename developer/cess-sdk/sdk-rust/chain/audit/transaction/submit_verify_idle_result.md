# Transaction: `submit_verify_idle_result`

```rust
#[allow(clippy::too_many_arguments)]
pub async fn submit_verify_idle_result(
        &self,
        total_prove_hash: BoundedVec<u8>,
        front: u64,
        rear: u64,
        accumulator: Accumulator,
        idle_result: bool,
        signature: BoundedVec<u8>,
        tee_puk: [u8; 32],
    ) -> Result<(TxHash, SubmitIdleVerifyResult), Error>
```

---

## Description

This transaction is submitted by a TEE node or an authorized verifier after completing the verification of a miner’s idle proof.
The verification process ensures that miners cannot cheat by submitting invalid or manipulated proofs.

Upon successful submission and validation:

The verification signature from the TEE is checked to confirm authenticity.

The verification result is recorded on-chain.

If both idle and service verification results pass, the miner becomes eligible for reward distribution according to the network’s reward mechanism.

---

## Parameters

| Name               | Type             | Description                                                                        |
| ------------------ | ---------------- | ---------------------------------------------------------------------------------- |
| `total_prove_hash` | `BoundedVec<u8>` | The hash of the total idle proof data used for verification.                       |
| `front`            | `u64`            | The lower boundary index of the proof range.                                       |
| `rear`             | `u64`            | The upper boundary index of the proof range.                                       |
| `accumulator`      | `Accumulator`    | A Merkle accumulator used to efficiently verify proof membership.                  |
| `idle_result`      | `bool`           | Indicates whether the idle proof passed (`true`) or failed (`false`) verification. |
| `signature`        | `BoundedVec<u8>` | The TEE-signed result confirming the validity of the proof.                        |
| `tee_puk`          | `[u8; 32]`       | The public key of the TEE node that signed the verification result.                |


---

## Returns

| Type                                   | Description                                                                                                                         |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `Ok((TxHash, SubmitIdleVerifyResult))` | Returns a tuple containing the transaction hash and the emitted event upon successful submission.                                   |
| `Err(Error)`                           | Returns an error if the verification result submission fails (e.g., invalid signature, TEE key mismatch, or transaction rejection). |


---

## Example

```rust
use cess_rust_sdk::{
    chain::audit::transaction::StorageTransaction as AuditTx,
    polkadot::{runtime_types::bounded_collections::bounded_vec::BoundedVec},
};


#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mnemonic = "bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice";
    let audit_tx = AuditTx::from_mnemonic(mnemonic);

    // Sample data
    let total_prove_hash = BoundedVec(vec![0xab, 0xcd]);
    let front = 100;
    let rear = 200;
    let accumulator : [u8; 256] = [0; 256];
    let idle_result = true;
    let signature = BoundedVec(vec![0xde, 0xad, 0xbe, 0xef]);
    let tee_puk: [u8; 32] = [1; 32];

    // Submit the idle proof verification result
    let (tx_hash, event) = audit_tx
        .submit_verify_idle_result(
            total_prove_hash,
            front,
            rear,
            accumulator,
            idle_result,
            signature,
            tee_puk,
        )
        .await?;

    println!("Transaction hash: {:?}", tx_hash);
    println!("Event: {:?}", event);

    Ok(())
}

```