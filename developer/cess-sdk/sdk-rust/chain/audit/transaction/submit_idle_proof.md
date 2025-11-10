# Transaction: submit_idle_proof

Submits an idle proof to the blockchain, verifying that the miner’s storage node was idle yet responsive during the audit challenge period.

---

## Function Definition
```rust
pub async fn submit_idle_proof(
    &self,
    idle_prove: BoundedVec<u8>,
) -> Result<(TxHash, SubmitIdleProof), Box<dyn std::error::Error>>

```

---

## Description

This asynchronous call allows a miner to submit their idle proof to the CESS chain.
The idle proof confirms that a miner’s node was properly operating during the challenge but had no active service requests at that time.

After submission, the chain updates the miner’s challenge status accordingly:

- The idle_prove field is set to Some().
- If both idle and service proofs are submitted, the value of CountedClear is reset.
- A TEE node is then randomly assigned to verify the submitted proof.


---

## Parameters
| Name         | Type             | Description                                                                                                                       |
| ------------ | ---------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `idle_prove` | `BoundedVec<u8>` | The bounded vector of bytes containing the proof of idleness. It must comply with the maximum proof size limits defined on-chain. |


---

## Returns
| Type                            | Description                                                                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `Ok((TxHash, SubmitIdleProof))` | Returns a tuple containing the transaction hash and a `SubmitIdleProof` structure representing the on-chain record of the submitted proof. |
| `Err(Error)`                    | Returned if proof submission fails due to network issues, invalid parameters, or runtime errors during transaction execution.              |


---

## Example

```rust
use cess_rust_sdk::{
    chain::audit::transaction::StorageTransaction as AuditTx,
    polkadot::runtime_types::bounded_collections::bounded_vec::BoundedVec,
};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mnemonic = "bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice";
    let audit_tx = AuditTx::from_mnemonic(mnemonic);

    // Example idle proof data (replace with actual proof bytes)
    let idle_prove: BoundedVec<u8> = BoundedVec(vec![1, 2, 3, 4, 5]);

    // Submit idle proof
    let (tx_hash, proof_record) = audit_tx.submit_idle_proof(idle_prove).await?;

    println!("Transaction hash: {:?}", tx_hash);
    println!("Idle proof submitted: {:?}", proof_record);

    Ok(())
}

```

--- 
## Notes

- Idle proofs must adhere to the on-chain proof size limit enforced by the runtime.
- Submitting an idle proof is mandatory for miners to remain eligible for continued participation in the network’s storage challenge process.
- The proof is verified by a TEE node, ensuring trustless and tamper-proof validation.
- **Security Recommendation:** Always use a hardware wallet or other secure signing method when submitting transactions to the CESS blockchain. This helps protect your private keys and prevents unauthorized access or malicious signing.