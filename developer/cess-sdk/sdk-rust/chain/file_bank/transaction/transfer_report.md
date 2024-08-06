# Call `transfer_report`

The `transfer_report` is used by miners to report a file to complete the storage transaction. After a file is declared, a certain number of miners need to report it to finally represent the file. A file will be divided into several parts, and each miner is responsible for storing a part of it, and uses `index` to identify it when reporting.


```rust
pub async fn transfer_report(
    &self,
    index: u8,
    deal_hash: &str,
) -> Result<(TxHash, TransferReport), Box<dyn std::error::Error>>
```