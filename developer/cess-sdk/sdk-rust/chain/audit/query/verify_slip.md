# Query: verify_slip

Verifies whether a challenge slip has been validated for a specific block and account. This query checks the verification status of a challenge slip, which contains the time node for verification and liquidation, as well as the corresponding miner's participation status.

---

## Function Definition
```rust
pub async fn verify_slip(
    block_number: u32,
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<bool>, Error>

```

## Description
This asynchronous query verifies whether the challenge slip for a given account at a specified block number has been validated. The verification status indicates whether the miner has successfully challenged and submitted proof, as well as the corresponding time node for verification and liquidation.

You can provide a block_hash to query the state at a specific block or omit it to query the most recent state of the chain.

---

## Parameters

| Name           | Type           | Description                                                                                                                                |
| -------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `block_number` | `u32`          | The block number for which the challenge slip verification is being checked.                                                               |
| `account`      | `&str`         | A valid **SS58-encoded account** representing the miner or storage provider.                                                               |
| `block_hash`   | `Option<H256>` | *(Optional)* Block hash for querying the state at a specific point in the blockchain. If omitted, the query returns the most recent state. |


---

## Returns

| Type              | Description                                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `Ok(Some(true))`  | The challenge slip has been validated for the given account at the specified block number. The miner's challenge has been verified.         |
| `Ok(Some(false))` | The challenge slip has not been validated for the given account at the specified block number. The miner’s challenge has not been verified. |
| `Ok(None)`        | No relevant verification data is available for the account at the specified block.                                                          |
| `Err(Error)`      | If the query fails due to invalid data, connectivity issues, or other errors.                                                               |


---

## Example

```rust
use cess_rust_sdk::chain::audit::query::StorageQuery as AuditQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";
    let block_number = 1_165_761;
    
    let result = AuditQuery::verify_slip(block_number, account, None).await?;

    match result {
        Some(true) => {
            println!("The challenge slip for account {} has been validated at block {}.", account, block_number);
        },
        Some(false) => {
            println!("The challenge slip for account {} has not been validated at block {}.", account, block_number);
        },
        None => {
            println!("No verification data found for the challenge slip of account {} at block {}.", account, block_number);
        }
    }

    Ok(())
}

```