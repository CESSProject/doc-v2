# Preface

The **CESS Rust SDK** enables developers to interact directly with the **CESS decentralized storage network**, offering APIs for querying blockchain data, performing transactions, managing user storage, and facilitating file operations.

This document serves as a starting point for setting up and using the SDK in your Rust environment.

---

## 1. Overview

With the Rust SDK, developers can:
- Connect to the CESS blockchain via remote nodes or custom endpoints.
- Execute signed transactions using mnemonics or key pairs.
- Query on-chain data such as territories, file storage information, and OSS (Object Storage Service) configurations.
- Build custom gateways and retrieval logic for decentralized file operations.

The SDK follows a modular architecture:
- **Chain Modules**: Contain blockchain-related logic divided into pallets such as `audit`, `balances`, `file_bank`, `oss` and `storage_handler`.
- **Gateway Module**: Provides network interfacing utilities and file transfer operations.
- **Retriever Module**: Handles file fetching, buffering, and disk operations.

Each module includes **query APIs** (read-only access) and **transaction APIs** (state-changing calls).

---

## 2. Installation

To use the CESS Rust SDK in your project, add the following dependency to your `Cargo.toml`:

```toml
[dependencies]
cess-rust-sdk = { git = "http://github.com/CESSProject/cess-rust-sdk.git", branch = "v0.8.0-premainnet" }
```

This pulls the latest version of the SDK directly from the official GitHub repository’s v0.8.0-premainnet branch.

## 3. Source Code

You can also clone the SDK repository manually:

```bash
git clone https://github.com/CESSProject/cess-rust-sdk.git
cd cess-rust-sdk
```

The SDK source includes:

- Example code for each module
- Predefined API provider implementations
- Query and transaction usage patterns for all supported pallets

## 4. Next Steps

To learn about each blockchain module and its APIs, continue to:

- [Chain Modules Overview](./chain/README.md)
- [Audit Queries and Transactions](./chain/audit/README.md)
- [Balances Transactions](./chain/balances/README.md)
- [File Bank Queries and Transactions](./chain/file_bank/README.md)
- [OSS Management](./chain/oss/README.md)
- [Storage Handler Operations](./chain/storage_handler/README.md)