# Call `submit_verify_service_result`


```rust
/// Submits the result of a service verification process to the blockchain.
///
/// This asynchronous function takes multiple parameters related to the verification of a service result,
/// submits the verification result to the blockchain, and returns the transaction hash along with a
/// `SubmitServiceVerifyResult` structure upon success.
///
/// # Parameters
/// - `service_result`: A `bool` indicating the result of the service verification.
/// - `signature`: A `BoundedVec<u8>` containing the signature to verify the service result.
/// - `service_bloom_filter`: A `BloomFilter` structure containing the service bloom filter data.
/// - `tee_puk`: A `[u8; 32]` array containing the TEE public key.
///
/// # Returns
/// - `Result<(TxHash, SubmitServiceVerifyResult), Box<dyn std::error::Error>>`: On success, returns a tuple containing
///   the transaction hash (`TxHash`) and a `SubmitServiceVerifyResult` struct. On failure, returns an error encapsulated
///   in a `Box<dyn std::error::Error>`.
pub async fn submit_verify_service_result(
    &self,
    service_result: bool,
    signature: BoundedVec<u8>,
    service_bloom_filter: BloomFilter,
    tee_puk: [u8; 32],
) -> Result<(TxHash, SubmitServiceVerifyResult), Box<dyn std::error::Error>>
```