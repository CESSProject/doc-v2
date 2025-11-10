# Proxy Re-Encryption

This module provides cryptographic primitives for **proxy re-encryption (PRE)**, enabling secure data sharing in decentralized environments without directly revealing private keys.

It is used internally by the **Retriever Gateway** to manage encrypted data re-encryption and access delegation between users.

---

## Overview

Proxy re-encryption allows an intermediary (proxy) to transform ciphertexts encrypted under one public key into ciphertexts decryptable by another key, **without learning the plaintext**.
This is achieved through the generation of **re-encryption keys** and the use of **capsules** that store elliptic curve points and scalars.

This module is built upon:

* [`curve25519-dalek`](https://docs.rs/curve25519-dalek/latest/curve25519_dalek/)
* [`schnorrkel`](https://docs.rs/schnorrkel/latest/schnorrkel/)
* [`sha2`](https://docs.rs/sha2/latest/sha2/)
* [`base64`](https://docs.rs/base64/latest/base64/)
* [`serde`](https://docs.rs/serde/latest/serde/)

---

## Core Structures

### `RetrieverError`

Custom error type covering JSON, Base64, length, and cryptographic validation errors.

### `Capsule`

Represents the cryptographic capsule used in proxy re-encryption:

* `e`: Ristretto curve point
* `v`: Ristretto curve point
* `s`: Scalar used for verification

Serialized capsules are Base64-encoded JSON objects.

---

## Main Functions

### `gen_re_encryption_key(ms_bytes, pk_b_bytes)`

Generates a **re-encryption key** and an **ephemeral public key**.

| Parameter    | Type    | Description                                         |
| ------------ | ------- | --------------------------------------------------- |
| `ms_bytes`   | `&[u8]` | Secret key bytes of the data owner (32 or 64 bytes) |
| `pk_b_bytes` | `&[u8]` | 32-byte public key of the delegate (receiver)       |

**Returns:** `(rk_bytes, pk_x_bytes)`
A tuple containing the re-encryption key and the ephemeral public key.

---

### `re_encrypt_key(capsule_json, rk_bytes)`

Performs re-encryption of a capsule using the provided re-encryption key.

| Parameter      | Type    | Description               |
| -------------- | ------- | ------------------------- |
| `capsule_json` | JSON    | Base64-encoded capsule    |
| `rk_bytes`     | `&[u8]` | 32-byte re-encryption key |

**Returns:** Re-encrypted capsule (Base64-encoded JSON)

---

### `decrypt_key(ms_bytes, capsule_json)`

Decrypts an **original** (non-re-encrypted) capsule to obtain the AES key.

---

### `decrypt_re_key(ms_bytes, pk_x_bytes, new_capsule_json)`

Decrypts a **re-encrypted** capsule using the recipient’s secret key and the sender’s ephemeral public key.
Derives a 32-byte AES symmetric key.

---

### `parse_scalar_bytes(b)`

Parses input into a scalar.
Accepts raw 32 bytes, Base64, or hex-encoded strings.

---

### `ristretto_point_from_pk(pk)`

Converts a Schnorrkel `PublicKey` into a Ristretto point.

---

## Algorithm Notes

* Uses **Ristretto255** for all elliptic curve operations.
* Verification uses the equality:
  `s * G == v + H(e, v) * e`
* All derived AES keys are generated via **SHA-256(point)**.
* Compatible with `curve25519_dalek` and `schnorrkel` key formats.

---
