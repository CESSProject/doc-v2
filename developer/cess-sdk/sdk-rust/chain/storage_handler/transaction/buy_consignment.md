# Call `buy_consignment`

Users can purchase the territories on consignment through transactions. After calling this transaction, the corresponding consignment order will be locked. During the lock-in period, users can cancel the purchase. When the lock-in period ends, the subsequent transaction content will be automatically executed.

```rust
/// Purchases a territory on consignment through a transaction. 
///
/// When this function is called, the corresponding consignment order will be locked.
/// During the lock-in period, users have the option to cancel the purchase. 
/// If the purchase is not canceled within the lock-in period, the subsequent 
/// transaction content will be automatically executed.
///
/// # Arguments
///
/// * `token` - A reference to a string slice that holds the authentication token.
/// * `rename` - A reference to a string slice that holds the name to be associated with the consignment order.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, BuyConsignment))` - A tuple containing the transaction hash and the `BuyConsignment` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///
pub async fn buy_consignment(
        &self,
        token: &str,
        rename: &str,
    ) -> Result<(TxHash, BuyConsignment), Box<dyn std::error::Error>>
```