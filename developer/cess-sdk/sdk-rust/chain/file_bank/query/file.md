# Query `file`

The `file` is used to query file metadata information.

```rust
/// Asynchronously retrieves the `FileInfo` associated with a given hash, and optionally
/// verifies it against a specified block hash.
///
/// # Parameters
/// - `hash`: A string slice that holds the hash identifier for the file.
/// - `block_hash`: An optional `H256` hash representing the block hash to verify the file against.
///
/// # Returns
/// A `Result` which is:
/// - `Ok(Some(FileInfo))` if the file is found and successfully retrieved.
/// - `Ok(None)` if the file is not found.
/// - `Err(Box<dyn std::error::Error>)` if there is an error during retrieval.
///
pub async fn file(
    hash: &str,
    block_hash: Option<H256>,
) -> Result<Option<FileInfo>, Box<dyn std::error::Error>>
```