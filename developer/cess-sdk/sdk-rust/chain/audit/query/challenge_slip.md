# Query: challenge_slip

Checks if a challenge slip exists for the given account at a specific block. A challenge slip records the deadline by which miners must challenge and submit their proofs. If the challenge slip exists for the given account at the specified block, the query returns true. Otherwise, it returns false.

---

## Function Definition

```rust
pub async fn challenge_slip(
    block_number: u32,
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<bool>, Error>
```

---

# Description
This asynchronous query checks whether a challenge slip exists for the given account at the specified block number. The challenge slip tracks the deadline for miners to submit their challenges and proofs for a given block.

You can provide a block_hash to query the state at a specific block or omit it to query the latest state.

---

## Parameters
| Name           | Type           | Description                                                                                                                                |
| -------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `block_number` | `u32`          | The block number for which the challenge slip is being checked.                                                                            |
| `account`      | `&str`         | A valid **SS58-encoded account** representing the miner or storage provider.                                                               |
| `block_hash`   | `Option<H256>` | *(Optional)* Block hash for querying the state at a specific point in the blockchain. If omitted, the query returns the most recent state. |


---

## Returns
| Type              | Description                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `Ok(Some(true))`  | A challenge slip exists for the given account at the specified block. The miner has a deadline to challenge and submit proof.          |
| `Ok(Some(false))` | No challenge slip exists for the given account at the specified block. No deadline or challenge submission is recorded for that block. |
| `Ok(None)`        | No relevant challenge slip data is available for the account.                                                                          |
| `Err(Error)`      | If the query fails due to invalid data, connectivity issues, or other errors.                                                          |

---

## Example

```rust
use cess_rust_sdk::chain::audit::query::StorageQuery as AuditQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";
    let block_number = 1_165_761;
    
    let result = AuditQuery::challenge_slip(block_number, account, None).await?;
    
    match result {
        Some(true) => {
            println!("A challenge slip exists for account {} at block {}.", account, block_number);
        },
        Some(false) => {
            println!("No challenge slip exists for account {} at block {}.", account, block_number);
        },
        None => {
            println!("No relevant challenge slip data found for account {} at block {}.", account, block_number);
        }
    }

    Ok(())
}

```