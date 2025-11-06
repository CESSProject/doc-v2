# Audit Module

The **Audit** module in the CESS Rust SDK provides interfaces to interact with the **audit pallet** on the CESS blockchain.  
This pallet manages **storage miner challenge verification**, including submission of proofs, verification results, and tracking challenge-related states.

Using this module, developers can:
- Query the audit state and miner challenge results.
- Submit storage or service proofs to the chain.
- Verify proof results for idle or service challenges.

---

## Query Interfaces

The following query APIs allow you to retrieve audit-related information directly from the blockchain:

- [challenge_snapshot](query/challenge_snapshot.md) - Retrieve challenge snapshot information for a specific miner or block.
- [counted_clear](query/counted_clear.md) - Query the count of cleared (successfully completed) challenges.
- [counted_service_failed](query/counted_service_failed.md) - Retrieve the count of failed service challenges for a miner.

---

## Transaction Interfaces

The following call (transaction) APIs allow miners or validators to interact with the audit system by submitting and verifying proofs:

- [submit_idle_proof](transaction/submit_idle_proof.md) - Submit proof of idle storage space to demonstrate reliability.
- [submit_service_proof](transaction/submit_service_proof.md) - Submit proof of completed service-related tasks.
- [submit_verify_idle_result](transaction/submit_verify_idle_result.md) - Submit the verification result for idle space proof.
- [submit_verify_service_result](transaction/submit_verify_service_result.md) - Submit the verification result for service proof.

---

### Example Usage

All query and transaction functions under the `audit` module can be accessed through:
```rust
use cess_rust_sdk::chain::audit::{AuditQuery, AuditTransaction};
```