# Call `update`

This function allows a registered OSS to update its information.

```rust
/// Update OSS Information
///
/// This function allows a registered OSS to update its information,
/// including its endpoint and domain. An OSS is a service provider that offers its services via a defined endpoint.
/// This function enables the OSS to modify its endpoint and provide an updated description of its services.
///
/// # Parameters
///
/// - `endpoint`: The new unique peer ID or endpoint that identifies the OSS and its services, represented as a byte array of length 38.
/// - `domain`: A bounded vector of up to 50 bytes representing the updated domain or description of the OSS's services.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the OSS information update.
/// - `OssUpdate`: A structure representing the result of the OSS update process.
///
pub async fn update(
    &self,
    endpoint: [u8; 38],
    domain: BoundedVec<u8>,
) -> Result<(TxHash, OssUpdate), Box<dyn std::error::Error>>
```