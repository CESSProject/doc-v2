# Transaction: `submit_service_proof`

Submits a service proof for verification on the CESS blockchain.
This function is used by storage miners to prove that they have correctly provided storage or data services during a given challenge period.

## Function Definition

```rust

pub async fn submit_service_proof(
    &self,
    service_prove: BoundedVec<u8>,
) -> Result<(TxHash, SubmitServiceProof), Error>

```

---

## Description

The submit_service_proof function allows miners to submit proof of service data to the CESS chain.
When the proof is submitted:

- The service_prove field of the miner’s on-chain state changes to Some().
- The miner’s challenge status is updated and re-evaluated.
- If both idle and service proofs have been successfully submitted, the CountedClear value is reset to 0.
- The blockchain randomly assigns a TEE node responsible for verifying the proof.

This is part of the Audit pallet logic that ensures miners are fulfilling their service responsibilities.

---

## Parameters

| Name            | Type             | Description                                                                                                                                                               |
| --------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `service_prove` | `BoundedVec<u8>` | The encoded service proof data, representing the miner’s proof of having served data correctly during a challenge. The vector is size-limited according to network rules. |


---

## Returns

| Type                               | Description                                                                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `Ok((TxHash, SubmitServiceProof))` | Returns a tuple with the transaction hash and the emitted `SubmitServiceProof` event upon successful submission. |
| `Err(Error)`                       | Returns an error if the transaction fails (e.g., invalid proof data, signing error, or chain execution failure). |


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

    // Encoded proof of service data
    let service_proof = BoundedVec(vec![1, 2, 3, 4, 5]);

    // Submit service proof transaction
    let result = audit_tx.submit_service_proof(service_proof).await?;

    println!("Transaction hash: {:?}", result.0);
    println!("Event: {:?}", result.1);

    Ok(())
}

```