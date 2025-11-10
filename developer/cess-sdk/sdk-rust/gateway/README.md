
# Gateway Module (DEOSS) — Deprecated

> **Status:** ❌ Deprecated  
> **Replaced By:** [`retriever`](../retriever/README.md)  
> **Module:** `cess_rust_sdk::gateway`

---

## Overview

The **Gateway (DEOSS)** module was the original CESS SDK component used to handle:
- Uploading and downloading files to and from CESS gateways
- Managing encrypted file operations (`upload_encrypt` / `download_encrypt`)
- Handling signed requests using on-chain accounts (`Signature`-based verification)
- Interacting with DeOSS services over HTTP

This module relied on `reqwest` for HTTP I/O, `tokio` for asynchronous operations, and basic Base58-encoded signatures for authorization.

### Deprecation Notice

This module is **deprecated** and is **no longer maintained**.  
It has been **replaced by the new [`retriever`](../retriever/README.md)** module, which introduces:

- End-to-end encryption and decryption using **proxy re-encryption (PRE)**  
- Safer and more extensible capsule management  
- Improved cryptographic primitives (`curve25519-dalek`, `schnorrkel`, `sha2`)  
- Cleaner async file retrieval interfaces  
- Direct blockchain verification and key derivation support

---

## Previous Functionality

### File Upload

```rust
let response = gateway::upload(
    "https://path_to_gateway_servers",
    "path/to/file.txt",
    "territory",
    "cess_account",
    "message",
    signed_msg,
).await?;
````

### Encrypted Upload

```rust
let response = gateway::upload_encrypt(
    "https://path_to_gateway_servers",
    "path/to/file.txt",
    "territory",
    "cess_account",
    "message",
    signed_msg,
    "cipher_text",
).await?;
```

### File Download

```rust
gateway::download(
    "https://path_to_gateway_servers",
    "file_id",
    "cess_account",
    "message",
    signed_msg,
    "save/path",
).await?;
```

---

## Migration to Retriever

The new [`retriever`](../retriever/README.md) module replaces Gateway (DEOSS) by offering secure and verifiable **proxy re-encryption** workflows, including:

* Generation of re-encryption keys
* Secure capsule management (Base64 + JSON encoding)
* AES-256 key derivation
* Cryptographic verification of delegation

---

## Notes

* The `gateway` module remains in the codebase **only for legacy support**.
* It will be **removed in future releases** of the CESS Rust SDK.
* Developers are strongly encouraged to migrate to `retriever`.

---