<!--
# General

| Term                                             | Definition |
| ------------------------------------------------ | ---------- |
| Block                                            | -          |
| Blockchain                                       | -          |
| Content IDentifier (CID)                         | -          |
| Continuous Availability Proof of Storage (CAPoS) | -          |
| Data Chunk                                       | -          |
| Data Fragment                                    | -          |
| Data Segment                                     | -          |
| Epoch                                            | -          |
| Era                                              | -          |
| File ID (FID)                                    | -          |
| Hash                                             | -          |
| Merkle Root                                      | -          |
| Multi-format Data Rights Confirmation (MDRC)     | -          |
| Peer-to-peer Network                             | -          |
| Proof of Data Reduplication and Recovery (PoDR²) | -          |
| Proxy Re-encryption Technology (PReT)            | -          |
| Random Rotational Selection (R²S)                | -          |
| Reputation Rotational Consensus (R²C)            | -          |
| Slot                                             | -          |
| Smart Contract                                   | -          |
| Tag                                              | -          |
| TEE Worker                                       | -          |
| Transaction Hash                                 | -          |
| Transaction                                      | -          |
| Trusted Execution Environment (TEE)              | -          |
| WebAssembly (Wasm)                               | -          |

-->

# DeOSS

| Term                                 | Definition |
| ------------------------------------ | ---------- |
| Territory                            | Territories are containers for storing objects. Each private key/account can manage up to many territories. Territory and the data within it can be transferred, sold and given away together |
| Decentralized Object Storage Service | Storage service that store user data as object and are accessible via CESS APIs. |
| Object                               | An Object is a basic unit for storing data. Unlike traditional file systems, objects do not have the hierarchical relational structure of a file directory. An object includes data, metadata, and fid. |
| Data | User's data includes but is not limited to high-definition pictures, audio, video, and backup files in websites and applications |
| FID | The file hash used to retrieve an object. The server and user can find an object through Fid without knowing the physical address of the data. It is the globally unique identifier (UID) of the object. |
| Metadata | Metadata is similar to data labels. There is no limit to the type and the number of items on the label, which can be various descriptions of objects. |

# Storage Miner

| Term              | Definition                                                                                                                                                                                             |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Endpoint          | A public endpoint for storage miner to communicate with CESS Network                                                                                                                                   |
| State             | <p><strong>Positive</strong>: Working fine<br><strong>Frozen</strong>: Out of staking<br><strong>Exit</strong>: Exiting CESS Network<br><strong>Offline</strong>: Being kicked out of CESS Network</p> |
| Staking Amount    | The amount of TCESS that the miner stakes                                                                                                                                                              |                                                                                                                                                                                                   |
| Staking Start     | The block height when the miner first run                                                                                                                                                              |                                                                                                                                                                                      |
| Debt Amount       | The debt amount of TCESS that the miner owes                                                                                                                                                              |                                                                                                                                                                                      |                                                                                                                                                                                                 |                                                                                                                                                                                      |
| Declaration Space | The declaration space on chain is auto set by the value of UseSpace after round up to the closest TB when the miner first run                                                                          |
| Validated Space   | The space validated to store user's data                                                                                                                                                               |
| Used Space        | The space used by storage network                                                                                                                                                                      |
| Locked Space      | The space locked by CESS Network                                                                                                                                                                       |
| Signature Account | The unique account in cess network, it prove the ownership of the storage miner                                                                                                                        |
| Staking Account   | The account that stakes TCESS, 4000 TCESS per TiB, It usually be the same as the Signature Account                                                                                                     |
| Earning Account   | The account that the reward is sending to                                                                                                                                                              |