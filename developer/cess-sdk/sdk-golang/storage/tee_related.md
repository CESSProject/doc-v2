This section describes how the storage node interacts with the tee worker. tee wokger is a node with [SGX functionality](https://en.wikipedia.org/wiki/Software_Guard_Extensions), a secure and trusted node in the network, and the authenticity of the storage miner's data is verified by the tee worker.

## PubkeysProvider Client
| Method | Description | Reference |
| ------ | ----------- | --------- |
| [NewPubkeyApiClient](https://github.com/CESSProject/p2p-go/blob/main/core/pubkey.go#L18) | Create a `PubkeysProvider` client | reservation |
| [GetIdentityPubkey](https://github.com/CESSProject/p2p-go/blob/main/core/pubkey.go#L26) | Get the identity pubkey of the tee worker | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/run.go#L1071) |
| [GetMasterPubkey](https://github.com/CESSProject/p2p-go/blob/main/core/pubkey.go#L47) | Get the master pubkey of the tee worker | reservation |
| [reservation](https://github.com/CESSProject/p2p-go/blob/main/core/pubkey.go#L68) | Get the podr2 pubkey of the tee worker | reservation |

## Podr2 Client
| Method | Description | Reference |
| ------ | ----------- | --------- |
| [NewPodr2ApiClient](https://github.com/CESSProject/p2p-go/blob/main/core/podr2.go#L18) | Create a `Podr2` client | reservation |
| [RequestGenTag](https://github.com/CESSProject/p2p-go/blob/main/core/podr2.go#L34) | Request tee worker calculation service file tag | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/calculate_tag.go#L290) |

## Podr2Verifier Client
| Method | Description | Reference |
| ------ | ----------- | --------- |
| [NewPodr2VerifierApiClient](https://github.com/CESSProject/p2p-go/blob/main/core/podr2.go#L26) | Create a `Podr2Verifier` client | reservation |
| [RequestBatchVerify](https://github.com/CESSProject/p2p-go/blob/main/core/podr2.go#L60) | Request tee worker to validate proof of service data | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/challenge_service.go#L579) |

## PoisVerifier Client
| Method | Description | Reference |
| ------ | ----------- | --------- |
| [NewPoisVerifierApiClient](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L26) | Create a `PoisVerifier` client | reservation |
| [RequestSpaceProofVerifySingleBlock](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L121) | Request tee worker to validate a single block of idle data | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/challenge_idle.go#L52) |
| [RequestVerifySpaceTotal](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L142) | Request tee worker to validate total idle data proofs | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/challenge_idle.go#L52) |

## PoisCertifier Client
| Method | Description | Reference |
| ------ | ----------- | --------- |
| [NewPoisCertifierApiClient](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L18) | Create a `PoisCertifier` client | reservation |
| [RequestMinerGetNewKey](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L34) | Get the public key for storing miner registrations | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/cmd/console/run.go#L1189) |
| [RequestMinerCommitGenChall](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L58) | Request tee worker to generate idle data challenge | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/attestation_idle.go#L46) |
| [RequestVerifyCommitProof](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L79) | Request tee worker to validate idle data challenge | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/attestation_idle.go#L46) |
| [RequestVerifyDeletionProof](https://github.com/CESSProject/p2p-go/blob/main/core/pois.go#L100) | Request tee worker to verify proof of deletion of idle data | [cess-miner](https://github.com/CESSProject/cess-miner/blob/main/node/replace_idle.go#L30) |