# Query: counted_clear

Retrieves the number of times a given account has failed to submit the required service proof for a challenge on the CESS chain. This is an important metric for evaluating a miner's consistency and reliability in fulfilling service requirements.

---

## Function Definition

```rust
pub async fn counted_clear(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<u8>, Error>
```
---

## Description

This asynchronous query fetches the count of cleared service failures (i.e., the number of times the miner failed to submit the required service proof for the challenge) for a specific account.
You can provide a block_hash to query the state at a particular block or omit it to query the current state of the chain.

---

## Parameters
|Name|Type|Description|
|--|--|--|
|account|&str|A valid SS58-encoded account representing the miner or storage provider to query.
|block_hash|Option<H256>|(Optional) Block hash for querying state at a specific point in the blockchain. If omitted, the query returns the most recent state.|

---

## Returns 
|Type|Description|
|--|--|
|Ok(Some(u8))|The number of times the specified account has failed to submit a service proof.|
|Ok(None)|No record of service failure count for the account.|
|Err(Error)|If the query fails due to invalid data, connectivity issues, or other errors.|

---

## Example
```rust
use cess_rust_sdk::chain::audit::query::StorageQuery as AuditQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXfg2SYcq85nyZ1U4ccx6QnAgSeLQB8aXZ2jstbw9CPGSmhXY";

    // Query the number of service failures
    let result = AuditQuery::counted_clear(account, None).await?;

    match result {
        Some(count) => println!("Account {} has {} cleared service failures.", account, count),
        None => println!("No record of cleared service failures found for account {}.", account),
    }

    Ok(())
}

```