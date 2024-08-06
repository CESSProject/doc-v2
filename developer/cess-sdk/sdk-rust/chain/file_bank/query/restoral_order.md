# Query `restoral_order`

The `restoral_order` is used to query the file recovery order, the storage miner can store more files by recovering the files in this order.

```rust
/// Asynchronously queries the restoral order for file recovery.
/// 
/// This function allows storage miners to determine the order in which files
/// should be recovered. By following this order, storage miners can optimize
/// their storage capacity and store more files efficiently.
/// 
/// # Parameters
/// 
/// - `hash`: A reference to a string slice that represents the identifier hash of the file.
/// - `block_hash`: An optional parameter specifying a block hash of type `H256`. 
///   If provided, the function will retrieve the restoral order as of the specified block.
/// 
/// # Returns
/// 
/// - `Result<Option<RestoralOrderInfo>, Box<dyn std::error::Error>>`: 
///   The function returns a `Result` which on success contains an `Option`:
///   - `Some(RestoralOrderInfo)` if the restoral order information is found.
///   - `None` if no restoral order information is available for the given hash.
///   On failure, it returns an error wrapped in a `Box<dyn std::error::Error>`.
/// 
pub async fn restoral_order(
    hash: &str,
    block_hash: Option<H256>,
) -> Result<Option<RestoralOrderInfo>, Box<dyn std::error::Error>>
```