


Through this transaction, users can consign their own territories, customize the price and then display it to other users. The consigned territory needs to be empty, and the status of the territory needs to be Active, and the remaining expiration time is greater than one day.

```rust
/// Consigns a user's territory, allowing them to set a custom price and display it to other users.
///
/// This transaction enables users to consign their own territories by customizing the price.
/// The consigned territory needs to be empty, its status must be `Active`, and the remaining expiration time must be greater than one day.
///
/// # Arguments
///
/// * `territory_name` - A reference to a string slice that holds the name of the territory to be consigned.
/// * `price` - The custom price for the consigned territory, specified in the smallest unit of the currency (e.g., wei).
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, Consignment))` - A tuple containing the transaction hash and the `Consignment` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn territory_consignment(
    &self,
    territory_name: &str,
    price: u128,
) -> Result<(TxHash, Consignment), Box<dyn std::error::Error>>
```