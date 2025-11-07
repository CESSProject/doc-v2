# Query: counted_clear

Retrieves the number of consecutive unsubmitted proofs for the miner's current challenge. This metric tracks the number of times the miner has failed to submit the required proof for the ongoing challenge. Once the miner submits the proof, the record will be reset to 0.

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

This asynchronous query retrieves the count of consecutive unsubmitted service proofs for a given account. Each time a miner fails to submit the required proof, the count is incremented. If the miner submits the proof, the count is reset to 0.

You can provide a block_hash to query the state at a specific block or omit it to get the most recent state of the chain.

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
|Ok(Some(u8))|The number of consecutive unsubmitted proofs for the given account.|
|Ok(None)|No record of unsubmitted proofs for the account, meaning the miner has either submitted proofs consistently or there is no relevant data.|
|Err(Error)|If the query fails due to invalid data, connectivity issues, or other errors.|

---

## Example
```rust
use cess_rust_sdk::chain::audit::query::StorageQuery as AuditQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXfg2SYcq85nyZ1U4ccx6QnAgSeLQB8aXZ2jstbw9CPGSmhXY";

    let result = AuditQuery::counted_clear(account, None).await?;

    match result {
        Some(count) => println!("Account {} has {} consecutive unsubmitted service proofs.", account, count),
        None => println!("No record of consecutive unsubmitted service proofs found for account {}.", account),
    }

    Ok(())
}

```