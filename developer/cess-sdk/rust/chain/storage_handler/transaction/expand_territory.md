# Call `expand_territory`

Transactions for expanding the user's territory can be called when the user expands the territory. The payment amount is determined by the remaining expiration time and the size of the territory the user needs to expand. Use the territory name to specify which territory you want to expand. (Less than one day will be counted as one day)

```rust
/// Expands the user's territory by increasing its size.
///
/// This transaction can be called when the user wants to expand their territory.
/// The payment amount is determined by the remaining expiration time and the size of the territory 
/// the user needs to expand. The `territory_name` parameter specifies which territory to expand.
/// Any remaining time less than one day will be counted as one full day.
///
/// # Arguments
///
/// * `territory_name` - A reference to a string slice that holds the name of the territory to expand.
/// * `gib_count` - The amount of space in GiB to be added to the territory.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, ExpansionTerritory))` - A tuple containing the transaction hash and the `ExpansionTerritory` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn expand_territory(
    &self,
    territory_name: &str,
    gib_count: u32,
) -> Result<(TxHash, ExpansionTerritory), Box<dyn std::error::Error>>
```