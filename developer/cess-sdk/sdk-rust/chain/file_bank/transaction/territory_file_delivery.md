# Call `territory_file_delivery`

The `territory_file_delivery` is used to transfer files from one territory to another.

```rust
/// Transfers files from one territory to another.
/// 
/// This function is used to facilitate the delivery of files between different territories.
/// It takes in the account details, the file hash, and the target territory where the file needs to be delivered.
/// 
/// # Parameters
/// 
/// - `account`: A reference to a string slice representing the account initiating the file transfer.
/// - `file_hash`: A reference to a string slice that represents the unique hash identifier of the file to be transferred.
/// - `target_territory`: A reference to a string slice representing the target territory token where the file will be delivered.
/// 
/// # Returns
/// 
/// - `Result<(TxHash, TerritoryFileDelivery), Box<dyn std::error::Error>>`: 
///   The function returns a `Result` which on success contains a tuple:
///   - `(TxHash, TerritoryFileDelivery)` where `TxHash` is the transaction hash of the file delivery and 
///     `TerritoryFileDelivery` contains the details of the file delivery process.
///   On failure, it returns an error wrapped in a `Box<dyn std::error::Error>`.
/// 

pub async fn territory_file_delivery(
    &self,
    account: &str,
    file_hash: &str,
    target_territory: &str,
) -> Result<(TxHash, TerritoryFileDelivery), Box<dyn std::error::Error>>
```