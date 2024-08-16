# Query `counted_service_failed`

```rust
/// Asynchronously retrieves the count of service failures for a given account.
/// 
/// This function queries the CESS chain to determine the number of times a 
/// service has failed for the specified account. An optional block hash can be 
/// provided to query the state at a specific block.
/// 
/// # Arguments
/// 
/// * `account` - A valid account identifier string for which the service failure count 
///   is to be retrieved.
/// * `block_hash` - An optional `H256` block hash. If provided, the function 
///   queries the state at the specified block. If not provided, the function 
///   queries the latest state.
/// 
/// # Returns
/// 
/// * `Result<Option<u32>, Box<dyn std::error::Error>>` - On success, returns an 
///   `Option` containing the count of service failures. 
///   - If the account has no recorded service failures, returns `Ok(None)`.
///   - On failure, returns an `Err` containing the error information.
/// 
pub async fn counted_service_failed(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<u32>, Box<dyn std::error::Error>> 

```

## Example

```rust
#[cfg(test)]
mod test {
    use super::*;

    #[tokio::test]
    async fn test_counted_service_failed() {
        let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";
        let block_hash = None;
        let csf = StorageQuery::counted_service_failed(account, block_hash)
            .await
            .unwrap_or(None);

        match csf {
            Some(value) => {
                assert_eq!(value, 0);
            },
            None => {
                assert!(csf.is_none());
            }
        }
    }
}
```