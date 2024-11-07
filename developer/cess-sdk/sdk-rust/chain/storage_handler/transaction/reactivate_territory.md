# Call `reactivate_territory`

When a user's territory expires, this transaction can be called to reactivate the territory. The territory status must be Expired to be reactivated. If it is in Frozen status, the territory renewal transaction should be called. 

```rust
/// Reactivates an expired territory for the user.
///
/// This transaction can be called to reactivate a territory when it has expired.
/// The territory status must be `Expired` to be reactivated. 
/// If the territory is in `Frozen` status, the territory renewal transaction should be called instead.
///
/// # Arguments
///
/// * `territory_name` - A reference to a string slice that holds the name of the territory to be reactivated.
/// * `days` - The number of days for which the territory is to be reactivated.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, ReactivateTerritory))` - A tuple containing the transaction hash and the `ReactivateTerritory` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn reactivate_territory(
    &self,
    territory_name: &str,
    days: u32,
) -> Result<(TxHash, ReactivateTerritory), Box<dyn std::error::Error>> 
```