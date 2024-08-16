# Query `challenge_snapshot`

```rust
/// Asynchronously retrieves the challenge snapshot data of the miner
/// 
/// This function queries the CESS chain to determine the challenge snapshot data of 
/// the miner. An optional block hash can be provided to query the state at a specific block.
/// 
/// # Arguments
/// 
/// * `account` - A valid account identifier string for which the challenge snapshot 
///   data is to be queried.
/// * `block_hash` - An optional `H256` block hash. If provided, the function queries 
///   the state at the specified block. If not provided, the function queries the latest state.
/// 
/// # Returns
/// 
/// * `Result<Option<u8>, Box<dyn std::error::Error>>` 
///   - On success, returns an `Option` containing the snapshot data for the challenge. 
///   - If the account has no records of the service proof count, returns `Ok(None)`.
///   - On failure, returns an `Err` containing the error information.
///

pub async fn challenge_snapshot(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<ChallengeInfo>, Box<dyn std::error::Error>>
```

## Example

```rust
#[cfg(test)]
mod test {
    use super::*;

    #[tokio::test]
    async fn test_challenge_snapshot() {
        let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";
        let block_hash = None;
        let csf = StorageQuery::challenge_snapshot(account, block_hash).await.unwrap_or(None);
        match csf {
            Some(value) => {
                // Verify the snapshot
                // For this example we are just printing it using dbg! macro
                dbg!(&value);
            },
            None => {
                assert!(csf.is_none());
            }
        }
    }
}
```