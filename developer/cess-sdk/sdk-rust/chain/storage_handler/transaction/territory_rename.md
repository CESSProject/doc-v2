# Call `territory_rename`

It is used by users to rename the territory, and it is required that no files are stored in the territory.

```rust
/// Renames a territory for the user.
///
/// This transaction allows a user to rename their territory. The territory must be empty 
/// (i.e., no files stored in it) for the rename operation to be successful.
///
/// # Arguments
///
/// * `old_territory_name` - A reference to a string slice that holds the current name of the territory to be renamed.
/// * `new_territory_name` - A reference to a string slice that holds the new name for the territory.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok(TxHash)` - The transaction hash of the successful territory rename operation.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
 pub async fn territory_rename(
    &self,
    old_territory_name: &str,
    new_territory_name: &str,
) -> Result<TxHash, Box<dyn std::error::Error>> 
```