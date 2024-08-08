# Call `renew_territory`

Used for user renewal of territory transactions. After the user has minted a territory, this transaction can be called to extend the expiration date of a certain territory. `days` indicates the number of days for renewal. The fee paid depends on the renewal time and the space size of the current territory. Use the territory name to specify which territory.

```rust
/// Renews the expiration date of a territory for the user.
///
/// After the user has minted a territory, this transaction can be called to extend the expiration date.
/// The fee paid depends on the renewal time and the space size of the current territory.
/// The `territory_name` parameter specifies which territory to renew.
///
/// # Arguments
///
/// * `territory_name` - A reference to a string slice that holds the name of the territory to be renewed.
/// * `days` - The number of days for the renewal period.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, RenewalTerritory))` - A tuple containing the transaction hash and the `RenewalTerritory` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn renew_territory(
    &self,
    territory_name: &str,
    days: u32,
) -> Result<(TxHash, RenewalTerritory), Box<dyn std::error::Error>>
```