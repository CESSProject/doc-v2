# Call `mint_territory`

Used for transactions where users mint territories. `gib_count` indicates the size of the territory the user wants to mint, in gib. The default expiration time for a territory is 30 days. Through the `territory_name` field, users can customize the name of the territory they mint, but the name of a territory owned by a user cannot be repeated.

```rust
/// Mints a new territory for the user.
///
/// This transaction is used when a user wants to mint a new territory. 
/// The size of the territory is specified by `gib_count` in GiB. 
/// The default expiration time for a territory is 30 days. 
/// Users can customize the name of the territory through the `territory_name` field, 
/// but the name of a territory owned by a user cannot be repeated.
///
/// # Arguments
///
/// * `gib_count` - The size of the territory to be minted, in GiB.
/// * `territory_name` - A reference to a string slice that holds the custom name of the territory to be minted.
/// * `days` - The duration in days for which the territory is to be valid. Defaults to 30 days if not specified.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, MintTerritory))` - A tuple containing the transaction hash and the `MintTerritory` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn mint_territory(
    &self,
    gib_count: u32,
    territory_name: &str,
    days: u32,
) -> Result<(TxHash, MintTerritory), Box<dyn std::error::Error>>
```