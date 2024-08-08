# Call `authorize`

This function allows an account to authorize another account as an operator. 
The operator is generally gateway account, allowing them to access storage for read and write operation.

```rust
/// Authorize an Operator
///
/// This function allows an account to authorize another account as an operator,
/// granting them specific permissions or access rights to perform actions on behalf of the authorizing account.
///
/// # Parameters
///
/// - `account`: The identifier of the account that will be authorized as an operator.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the authorization of the operator.
/// - `Authorize`: A structure representing the result of the authorization process.
///
pub async fn authorize(
        &self,
        account: &str,
    ) -> Result<(TxHash, Authorize), Box<dyn std::error::Error>> 
```