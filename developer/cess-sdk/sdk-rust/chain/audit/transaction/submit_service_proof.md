# Call `submit_service_proof`

```rust
/// Submits a proof of service to the blockchain.
///
/// This asynchronous function takes a bounded vector of bytes representing the service proof,
/// submits it to the blockchain, and returns the transaction hash along with a
/// `SubmitServiceProof` structure upon success.
///
/// # Parameters
/// - `service_prove`: A `BoundedVec<u8>` that contains the proof of service data. This vector is bounded to ensure
///   that the proof size does not exceed predefined limits.
///
/// # Returns
/// - `Result<(TxHash, SubmitServiceProof), Box<dyn std::error::Error>>`: On success, returns a tuple containing
///   the transaction hash (`TxHash`) and a `SubmitServiceProof` struct. On failure, returns an error encapsulated in
///   a `Box<dyn std::error::Error>`.
///
pub async fn submit_service_proof(
    &self,
    service_prove: BoundedVec<u8>,
) -> Result<(TxHash, SubmitServiceProof), Box<dyn std::error::Error>>
```