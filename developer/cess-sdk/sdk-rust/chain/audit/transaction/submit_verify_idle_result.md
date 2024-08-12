# Call `submit_verify_idle_result`

```rust
/// Submits the result of an idle verification process to the blockchain.
///
/// This asynchronous function takes multiple parameters related to the verification process,
/// submits the verification result to the blockchain, and returns the transaction hash along
/// with a `SubmitIdleVerifyResult` structure upon success.
///
/// # Parameters
/// - `total_prove_hash`: A `BoundedVec<u8>` containing the hash of the total proof.
/// - `front`: A `u64` value representing the pre-offset in the proof range.
/// - `rear`: A `u64` value representing the post-offset in the proof range.
/// - `accumulator`: An `Accumulator` structure containing the accumulated data.
/// - `idle_result`: A `bool` indicating the result of the idle verification.
/// - `signature`: A `BoundedVec<u8>` containing the signature from TEE
/// - `tee_puk`: A `[u8; 32]` array containing the TEE public key.
///
/// # Returns
/// - `Result<(TxHash, SubmitIdleVerifyResult), Box<dyn std::error::Error>>`: On success, 
///   returns a tuple containing the transaction hash (`TxHash`) and a `SubmitIdleVerifyResult` 
///   struct. On failure, returns an error encapsulated in a `Box<dyn std::error::Error>`.
///
pub async fn submit_verify_idle_result(
    &self,
    total_prove_hash: BoundedVec<u8>,
    front: u64,
    rear: u64,
    accumulator: Accumulator,
    idle_result: bool,
    signature: BoundedVec<u8>,
    tee_puk: [u8; 32],
) -> Result<(TxHash, SubmitIdleVerifyResult), Box<dyn std::error::Error>> 
```