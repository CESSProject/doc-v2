This section introduces how transactions are stored and how CESS business-related meta information is stored.

In the Substrate framework, a simple key-value pair is used to store data, which is the basic implementation for database support and modification. All of the high level storage abstractions of Substrate are based on this simple key-value pair implementation.

# Key-value Database

The storage database of Substrate is based on RocksDB implementation, which is a persistent key-value storage database suitable for fast storage scenarios and supports the Parity database currently in its beta version.

This database is used for all Substrate components that require persistent storage:

- Substrate client
- Substrate light node
- Off-chain worker

# Data Storage Used by CESS

CESS network uses a more advanced database abstraction provided by Substrate.

[**`StorageValue` Type**](https://paritytech.github.io/substrate/master/frame_support/storage/types/struct.StorageValue.html) is used to store any single value. The following are common cases:

- Single value of meta information
- Single structure object
- Collection of single related items

If a list is used to store items, you need to pay attention to the length of the list. Very large lists or structures will incur storage costs. Running large lists or structures at runtime may affect network performance, or even stop producing blocks. CESS network uses this type to store some public certificates or public keys for TEE workers. For specific usage, refer to [here](https://paritytech.github.io/substrate/master/frame_support/storage/trait.StorageValue.html#required-methods).

[**`StorageMap` Type**](https://paritytech.github.io/substrate/master/frame_support/storage/types/struct.StorageMap.html) stores the mappings of single key to its values. The mapping storage is applicable to the case where the storage items are randomly accessed, rather than sequential iteration on the whole,  such as the balance value of a specific account. It is used to store the meta information of nodes and files in CESS.

[**`StorageDoubleMap` Type**](https://paritytech.github.io/substrate/master/frame_support/storage/types/struct.StorageDoubleMap.html) stores the mappings of two keys to their values. One advantage of this storage is that it can effectively delete all items with the first key. In CESS, it is used for storing the meta information of idle files stored in a storage node.

[**`StorageNMap` Type**](https://paritytech.github.io/substrate/master/frame_support/storage/types/struct.StorageNMap.html) stores the mappings of any key to its values. Currently, CESS does not use this type of storage.

For mapping type storage, you can customize the mapping algorithm based on the sensitivity of the stored data. It provides the flexibility to choose between algorithms with fast computation speed or algorithms with higher security but slower computation speed.

# Storage Query

Based on the blockchain constructed with Substrate, any node can expose a remote procedure calling (RPC) server. When accessing a storage object through RPC, you only need to provide the primary key related to the object.

In runtime logic, developers need to make a rigorous judgment on the storage information and how to use it efficiently.

# Transaction Storage

Runtime involves the underlying key-value database and the storage overlay layer in memory. They will track the states and changes of data until the value is submitted to the underlying database. By default, before the runtime function submits the value to the primary storage overlay layer, the changed state will be written to the transaction storage layer in memory separately. If the transaction cannot complete due to an error, changes in the transaction storage layer will be discarded.

You can add a transaction storage layer by declaring a [`#[transactional]`](https://paritytech.github.io/substrate/master/frame_support/attr.transactional.html) macro. Of course, this will consume more computer resources. But for security reasons, CESS makes explicitly declaration for each transaction and adds a transaction storage layer. In the future version of Substrate, the transaction storage layer is added by default.
