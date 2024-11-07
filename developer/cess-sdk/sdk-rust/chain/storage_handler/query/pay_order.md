# Query `pay_order`

Used to store payment orders for purchasing space for other users.

```rust
/// Retrieve Payment Order Details
///
/// This function retrieves the details of a payment order used to purchase space for other users.
/// It allows querying the payment order information associated with the specified order hash.
///
/// # Parameters
///
/// - `order_hash`: The hash that uniquely identifies the payment order.
/// - `block_hash`: An optional parameter specifying the block hash at which to perform the query. If `None`, the latest block is used.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains an `Option<OrderInfo>`:
/// - `Some(OrderInfo)`: The payment order information associated with the specified order hash.
/// - `None`: If no payment order information is found for the specified order hash.
///
pub async fn pay_order(
    order_hash: &str,
    block_hash: Option<H256>,
) -> Result<Option<OrderInfo>, Box<dyn std::error::Error>> 
```