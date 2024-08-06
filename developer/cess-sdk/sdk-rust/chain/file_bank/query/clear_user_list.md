# Query `clear_user_list`

```rust
pub async fn clear_user_list(
    block_hash: Option<H256>,
) -> Result<Option<BoundedVec<(AccountId32, BoundedVec<u8>)>>, Box<dyn std::error::Error>>
```