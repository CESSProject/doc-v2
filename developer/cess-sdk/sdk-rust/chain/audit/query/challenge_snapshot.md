# Query: challenge_snapshot

Retrieves the challenge snapshot data for a specific account. This snapshot contains metadata about the ongoing or most recent challenge for the miner, including various details such as challenge state, failure count, and other associated information.

---

## Function Definition

```rust
pub async fn challenge_snapshot(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<ChallengeInfo>, Error>

```

---

## Description
This asynchronous query retrieves the challenge snapshot for the given account. The snapshot provides critical information about the miner's current challenge, such as challenge status, the number of failed attempts, and other related data.

You can provide a block_hash to query the state at a specific block or omit it to get the most recent state of the chain.

---
## Parameters

| Name         | Type           | Description                                                                                                                            |
| ------------ | -------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `account`    | `&str`         | A valid **SS58-encoded account** representing the miner or storage provider to query.                                                  |
| `block_hash` | `Option<H256>` | *(Optional)* Block hash for querying state at a specific point in the blockchain. If omitted, the query returns the most recent state. |

---

## Returns
| Type                      | Description                                                                                                                               |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `Ok(Some(ChallengeInfo))` | The challenge snapshot for the specified account. Contains metadata like challenge state, failure count, and other details.               |
| `Ok(None)`                | No challenge snapshot available for the account, meaning the miner is not currently involved in a challenge or there is no relevant data. |
| `Err(Error)`              | If the query fails due to invalid data, connectivity issues, or other errors.                                                             |


---

## Example

```rust
use cess_rust_sdk::chain::audit::query::StorageQuery as AuditQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXgaaRfFRcVtA3MSPJfcyCUw2FD7hMFeFaj7EGyF795Ep5VcG";

    let result = AuditQuery::challenge_snapshot(account, None).await?;

    match result {
        Some(snapshot) => {
            println!("Challenge Snapshot for account {}: {:?}", account, snapshot);
        },
        None => {
            println!("No challenge snapshot found for account {}.", account);
        }
    }
    Ok(())
}
```