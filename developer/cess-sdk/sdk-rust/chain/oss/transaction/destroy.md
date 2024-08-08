# Call `destroy`

This function allows a registered OSS to voluntarily destroy its registration.

```rust
/// Destroy OSS Registration
///
/// This function allows a registered OSS to voluntarily destroy its registration,
/// effectively unregistering from the system. Once an OSS is unregistered,
/// it will no longer be recognized as a valid service provider.
///
/// # Parameters
///
/// This function does not take any parameters.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the destruction of the OSS registration.
/// - `OssDestroy`: A structure representing the result of the OSS destruction process.
///
/// 

pub async fn destroy(&self) -> Result<(TxHash, OssDestroy), Box<dyn std::error::Error>>
```