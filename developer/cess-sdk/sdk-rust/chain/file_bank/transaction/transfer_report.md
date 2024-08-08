# Call `transfer_report`

The `transfer_report` is used by miners to report a file to complete the storage transaction. After a file is declared, a certain number of miners need to report it to finally represent the file. A file will be divided into several parts, and each miner is responsible for storing a part of it, and uses `index` to identify it when reporting.


```rust
/// Reports a file to complete the storage transaction. This function is used by miners to report their part of the file storage,
/// which is necessary for the file to be fully represented and declared on the chain.
///
/// # Parameters
///
/// - `index`: The index of the part of the file that the miner is responsible for. This helps to identify which segment of the file is being reported.
/// - `deal_hash`: The unique hash associated with the storage deal for the file.
///
/// # Returns
///
/// Returns a `Result` that, on success, contains a tuple with:
/// - `TxHash`: The transaction hash associated with the reporting of the file storage.
/// - `TransferReport`: A structure representing the result of the file transfer reporting.
///
pub async fn transfer_report(
    &self,
    index: u8,
    deal_hash: &str,
) -> Result<(TxHash, TransferReport), Box<dyn std::error::Error>>
```