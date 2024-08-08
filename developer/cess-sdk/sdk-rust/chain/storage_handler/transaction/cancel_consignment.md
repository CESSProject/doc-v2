# Call `cancel_consignment`

This transaction is used to remove the consignments that the current user is currently consigning, but the consignment needs to be unlocked.

```rust
/// Cancels a consignment that the current user is currently consigning.
///
/// This function removes the consignment order for the specified territory name, 
/// but the consignment needs to be unlocked before it can be canceled.
///
/// # Arguments
///
/// * `territory_name` - A reference to a string slice that holds the name of the territory
///                      associated with the consignment order to be canceled.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, CancelConsignment))` - A tuple containing the transaction hash and the `CancelConsignment` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn cancel_consignment(
    &self,
    territory_name: &str,
) -> Result<(TxHash, CancleConsignment), Box<dyn std::error::Error>> 
```