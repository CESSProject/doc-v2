# Retriever API

The retriever API provides client-side functionality for interacting with the CESS Gateway Service and handling proxy re-encryption operations.
It supports secure file uploads/downloads, delegated data access, and re-encryption workflows between users and retriever nodes.

This module is designed to integrate with decentralized storage environments where users retain encryption control over their data while allowing authorized access through re-encryption keys.

---

## Modules
| Module                                          | Description                                                                                                                                 |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| [`gateway`](gateway.md)                         | Provides asynchronous APIs for interacting with the CESS retriever gateway - file upload/download, token generation, and batch transfers.             |
| [`proxy_re_encryption`](proxy_re_encryption.md) | Implements cryptographic proxy re-encryption logic, key derivation, and capsule transformation based on the Ristretto curve and Schnorrkel. |

---

## Features

- Proxy Re-Encryption - Enable delegated decryption without exposing private keys.

- Gateway Integration - Upload, download, and batch transfer files via authenticated endpoints.

- Access Token Management - Generate signed gateway tokens using on-chain account keys.

- Chunked Uploads - Support large file uploads with resumable batch operations.

- Secure Authentication - Wrap messages for signing, using standard <Bytes>…</Bytes> formatting.

## Related Documentation

[Gateway Module Documentation] (./gateway.md)

[Proxy Re-Encryption Module Documentation] (proxy_re_encryption.md)