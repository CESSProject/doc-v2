# Query: `counted_service_failed`

Retrieves the total number of **service failures** recorded for a specific account on the CESS chain.  
This query is part of the **Audit pallet** and helps determine how often a storage miner has failed service verification tasks.

---

## Function Definition

```rust
pub async fn counted_service_failed(
    account: &str,
    block_hash: Option<H256>,
) -> Result<Option<u32>, Error>
```
---

## Description
This asynchronous method queries the CESS blockchain for the count of failed service challenges associated with a specific account.
Optionally, a block_hash can be provided to inspect the state of the chain at a particular block height.

---

### Parameters
| Name | Type | Description |
|---|---|---|
| account| &str | A valid SS58-encoded account representing the miner or user to query.|
|block_hash|Option<H256>|(Optional) Block hash used to query historical state. If omitted, the latest state is queried.|

---

### Returns
|Type| Description|
|---|---|
|Ok(Some(u32))|Returns the number of recorded service failures for the given account.|
|Ok(None)|No record of service failures exists for the account.|
|Err(Error)|The query failed due to invalid parameters or connection issues.|


## Example

```rust
#[cfg(test)]
use cess_rust_sdk::chain::audit::query::StorageQuery;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let account = "cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB";

    // Query the number of service failures
    let result = StorageQuery::counted_service_failed(account, None).await?;

    match result {
        Some(count) => println!("Account {} has {} service failures.", account, count),
        None => println!("No service failure record found for account {}.", account),
    }

    Ok(())
}

```