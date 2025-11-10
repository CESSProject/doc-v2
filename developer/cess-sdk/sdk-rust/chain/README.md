# Chain Modules

This section introduces the core chain modules of the **CESS Rust SDK**.  
Each module corresponds to a specific on-chain pallet and provides both **query** and **transaction** interfaces to interact with the CESS blockchain.

These modules enable you to fetch chain state, submit transactions, and manage storage, files, and operational logic across the network.

---

## Modules Overview

- **[Audit](audit/README.md)**  
  Manage and query auditing operations such as proof submissions, verification snapshots, and challenge results.

- **[Balances](balances/README.md)**  
  Handle balance-related queries and token transfer operations (to be expanded in later versions).

- **[File Bank](file_bank/README.md)**  
  Manage buckets, files, and restoration orders. Includes APIs for uploading, deleting, and retrieving user file data.

- **[OSS (Object Storage Service)](oss/README.md)**  
  Register and authorize OSS accounts to perform file-related operations on behalf of users. Provides APIs for OSS registration, updates, and permissions.

- **[Storage Handler](storage_handler/README.md)**  
  Manage territories, storage space, and related financial transactions such as consignment sales, purchases, and renewals.

---

Each subdirectory includes:
- **Query APIs** - for retrieving blockchain state.  
- **Transaction APIs** - for performing signed actions on-chain.

Continue to the respective module to explore available functions and usage examples.
