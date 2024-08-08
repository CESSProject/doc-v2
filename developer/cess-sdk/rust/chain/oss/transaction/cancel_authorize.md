# Call `cancel_authorize`

This function allows an account to cancel the authorization of another account (operator), 
revoking the operator's permissions or access rights to perform actions on behalf of the authorizing account.

```rust
/// Cancel Authorization of an Operator
///
/// This function allows an account to revoke the authorization of an operator,
/// removing their permissions or access rights to perform actions on behalf of the authorizing account.
///
/// # Parameters
///
/// - `account`: The identifier of the account whose operator authorization will be revoked.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the cancellation of the operator authorization.
/// - `CancelAuthorize`: A structure representing the result of the authorization cancellation process.
///
pub async fn cancel_authorize(
    &self,
    account: &str,
) -> Result<(TxHash, CancelAuthorize), Box<dyn std::error::Error>>
```    