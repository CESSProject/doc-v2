# Query `counted_clear`

```rust
/// Asynchronously retrieves the number of times the miner has not submitted the service proof.
/// 
/// This function queries the CESS chain to determine the number of consecutive times
/// the miner has failed to submit the service proof. An optional block hash can be 
/// provided to query the state at a specific block.
/// 
/// # Arguments
/// 
/// * `account` - A valid account identifier string for which the count is to be queried.
/// * `block_hash` - An optional `H256` block hash. If provided, the function queries 
///   the state at the specified block. If not provided, the function queries the latest state.
/// 
/// # Returns
/// 
/// * `Result<Option<u8>, Box<dyn std::error::Error>>` 
///   - On success, returns an `Option` containing the count of times the miner failed 
///     to submit the service proof. Ok<Some<0>> is returned if miner is functioning normally. 
///   - If the account has no records of the service proof count, returns `Ok(None)`.
///   - On failure, returns an `Err` containing the error information.
///
pub async fn counted_clear(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<u8>, Box<dyn std::error::Error>>

```

## Example

```rust
#[cfg(test)]
mod test {
    use super::*;

    #[tokio::test]
    async fn test_counted_clear() {
        let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";
        let block_hash = None;
        let csf = StorageQuery::counted_clear(account, block_hash).await.unwrap_or(None);
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