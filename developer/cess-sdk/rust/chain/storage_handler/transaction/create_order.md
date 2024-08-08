# Call `create_order`

Create a transaction for a payment order. Create a payment order to purchase territory for `target_acc`. Specify the space size and time of the territory to be paid for by `gib_count` and `days`. `order_type` specifies whether the order type is purchase, expansion or renewal.

```rust
/// Creates a payment order to purchase, expand, or renew a territory for a specified account.
///
/// This function creates a payment order for the `target_acc` to purchase a territory.
/// The space size and duration of the territory are specified by `gib_count` and `days`, respectively.
/// The `order_type` parameter specifies whether the order is for a purchase, expansion, or renewal.
///
/// # Arguments
///
/// * `target_acc` - A reference to a string slice that holds the target account for the payment order.
/// * `territory_name` - A reference to a string slice that holds the name of the territory.
/// * `order_type` - The type of order (purchase, expansion, or renewal).
/// * `gib_count` - The amount of space in GiB to be purchased.
/// * `days` - The duration in days for which the territory is to be purchased.
/// * `expired` - The expiration timestamp for the order.
///
/// # Returns
///
/// This function returns a `Result` which is:
/// * `Ok((TxHash, CreatePayOrder))` - A tuple containing the transaction hash and the `CreatePayOrder` struct.
/// * `Err(Box<dyn std::error::Error>)` - An error if the transaction fails.
///

pub async fn create_order(
    &self,
    target_acc: &str,
    territory_name: &str,
    order_type: OrderType,
    gib_count: u32,
    days: u32,
    expired: u32,
) -> Result<(TxHash, CreatePayOrder), Box<dyn std::error::Error>>
```