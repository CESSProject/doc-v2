# Retriever Gateway API Client

The **Gateway API Client** provides asynchronous utilities to interact with the **CESS Gateway Service**, handling secure file upload/download, access token generation, proxy re-encryption, and batch uploads for large data transfers.

This module is a key component of the `retriever` crate and is designed for decentralized or encrypted storage systems that integrate **proxy re-encryption** and **territory-based access control**.

---

## Overview

The Gateway API allows clients to:

* Authenticate via signed tokens
* Upload or batch-upload files with optional encryption
* Download data, optionally using proxy re-encryption
* Retrieve capsules and gateway public keys
* Delegate decryption access securely using re-encryption keys

---

## Key Components

### **Endpoints**

Each API endpoint is represented by an `Endpoint` enum variant:

* `GenToken` - Generate access tokens
* `UploadFile` - Upload a single file
* `BatchUpload` - Upload a chunk in a batch session
* `BatchRequest` - Initialize or resume a batch upload
* `GetFile` - Download a file or segment
* `ReEncrypt` - Re-encrypt a data capsule
* `Capsule` - Retrieve stored capsule and gateway public key

---

### **Core Data Structures**

| Struct               | Description                                                   |
| -------------------- | ------------------------------------------------------------- |
| `Response<T>`        | Generic wrapper for gateway responses (`code`, `msg`, `data`) |
| `ReencryptReq`       | Proxy re-encryption request body (`did`, `capsule`, `rk`)     |
| `FileInfo`           | Metadata describing a file stored in the gateway              |
| `BatchFilesInfo`     | Metadata used to initialize or resume batch uploads           |
| `BatchUploadResp`    | Response returned after each successful batch upload          |
| `UploadFileOpts`     | Configuration options for uploading a file                    |
| `BatchUploadRequest` | Structure defining batch upload initialization                |

---

## Main Functions

### **`send_http_request()`**

A low-level helper for sending HTTP requests with headers and optional bodies, returning raw response bytes.
Used internally by all higher-level functions.

---

### **`proxy_re_encrypt()`**

Performs **proxy re-encryption** of an encrypted capsule for a new recipient.
This allows the gateway to transform ciphertexts without accessing plaintext data.
See also: [`proxy_re_encryption.md`](proxy_re_encryption.md)

---

### **`download_data()`**

Downloads an encrypted file or segment from the gateway.
If `capsule`, `rk`, and `pkx` are provided, the gateway can re-encrypt data dynamically before delivery.

---

### **`get_precapsule_and_pubkey()`**

Retrieves the capsule and gateway public key (`pubkey`) for a file.
This information is used during proxy re-encryption to prepare shared access.

---

### **`gen_gateway_access_token()`**

Generates an **access token** for authenticating gateway operations.
Requires a signed message in `<Bytes>…</Bytes>` format produced by [`wrap_message_for_signing()`](#wrap_message_for_signing).

> **Security Note:**
> Always sign messages with a hardware wallet or trusted key manager to protect private keys.

---

### **`upload_file()`**

Uploads a single file to the gateway, with options for asynchronous upload, proxy routing, and encryption.

---

### **`request_batch_upload()`**

Initializes a batch upload session for large files, preparing metadata such as filename, total size, and territory.

---

### **`batch_upload_file()`**

Uploads a file **chunk** within a batch session.
Each chunk is defined by a byte range `[start, end)` and sent using a multipart form.

---

### **`wrap_message_for_signing()`**

Wraps a SHA-256 hash of a message in `<Bytes>` tags, preparing it for signing using Substrate-compatible tools.
This ensures message consistency across signing methods and wallets.

---

## Example

For full working examples demonstrating file upload, download, and batch upload with token authentication and signature verification, see the official test file:

[**Gateway API Example**](https://github.com/CESSProject/cess-rust-sdk/blob/main/examples/src/retriever.rs)

