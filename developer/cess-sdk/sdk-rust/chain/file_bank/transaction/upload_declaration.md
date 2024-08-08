# Call `upload_declaration`

The `upload_declaration` is used to generate file storage orders.

```rust
/// Generates file storage orders for a given file.
/// 
/// This function is used to create storage orders by declaring the details of the file,
/// including its hash, segment list, user information, and size. The resulting storage
/// orders facilitate the process of storing the file in the system.
/// 
/// # Parameters
/// 
/// - `file_hash`: A reference to a string slice that represents the unique hash identifier of the file.
/// - `segment_list`: A `BoundedVec` containing the segments of the file. Each segment is described by the `SegmentList` struct.
/// - `user_brief`: A `UserBrief` struct that contains brief information about the user making the declaration.
/// - `file_size`: A `u128` value representing the size of the file in bytes.
/// 
/// # Returns
/// 
/// - `Result<(TxHash, UploadDeclaration), Box<dyn std::error::Error>>`: 
///   The function returns a `Result` which on success contains a tuple:
///   - `(TxHash, UploadDeclaration)` where `TxHash` is the transaction hash of the upload declaration and 
///     `UploadDeclaration` contains the details of the generated file storage order.
///   On failure, it returns an error wrapped in a `Box<dyn std::error::Error>`.
/// 

pub async fn upload_declaration(
    &self,
    file_hash: &str,
    segment_list: BoundedVec<SegmentList>,
    user_brief: UserBrief,
    file_size: u128,
) -> Result<(TxHash, UploadDeclaration), Box<dyn std::error::Error>> 
```