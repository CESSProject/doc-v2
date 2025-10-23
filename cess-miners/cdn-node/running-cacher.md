# Running the Cacher

Cacher nodes are edge caching nodes distributed across the CD²N network, designed to operate efficiently with minimal resources. They are suitable for deployment on various devices including personal computers, servers, smartphones, and Raspberry Pi. The primary functions of Cacher nodes include providing or caching user data for Retrievers in CD²N, creating value through contributions of cache storage and bandwidth resources. Cacher nodes currently operate in three modes:
1. Receiving user data shards from gateways and distributing them to connected storage nodes, accelerating persistent storage processes;
2. Retrieving data shards from local cache, storage nodes, or alternative sources (e.g., IPFS) to fulfill data requests from Retriever nodes;
3. Storing user data offloaded from Retriever nodes in local cache for rapid response, enabling dynamic scaling of Retriever node cache capacity;

Typical deployment scenarios for Cacher nodes include:
1. Independent operation on personal devices to earn rewards by caching offloaded data from Retriever nodes;
2. Co-deployment with storage nodes to enhance mining rewards through data provisioning and earn additional income by serving data from storage nodes to Retrievers;
3. Integration with external platforms (e.g., IPFS nodes) to facilitate cross-platform resource interoperability and earn rewards by serving data from these platforms to Retrievers;

Cacher node rewards are calculated based on verifiable data volume contributed to Retriever nodes. Rewards are distributed periodically according to the Proof of Traffic (PoT) protocol, with properly configured nodes automatically claiming earnings each operational cycle.

## Hardware Requirements

- Processor: 2.0 GHz or higher
- Memory: 2 GB or higher
- Storage: 32 GB or higher
- Bandwidth: 10 Mbps or higher
- OS: Ubuntu, CentOS
- Network: TCP/IP support

## Account Preparation

Operating a Cacher node requires two Ethereum wallet accounts:
1. **Node Operational Account**: For node registration and reward collection
2. **Token Account**: Holds the NFT access certificate for CD²N network participation

Key considerations:
- Both accounts interact with smart contracts on CESS consensus nodes' EVM
- Each NFT token can only authorize one Cacher node
- Testnet phases: 
  - Early phase: No token required
  - Mid phase: Tokens distributed by CESS community
  - Final phase: Test contract activation

Setup steps:
1. Create Operational Account: 
   - Generate Ethereum wallet
   - Fund with $CESS for transaction fees
2. Create Token Account:
   - Generate Ethereum wallet
   - Acquire token via purchase or community distribution
3. Generate Token Authorization:
   - Use CD²N signing tool with token account's private key
   - Sign node operational account and token details
   - This signature serves as proof of token-node binding

## Configuration File
When leaving account, token, and signature fields empty, the node operates in altruistic mode - contributing resources without financial rewards. This mode gains gateway trust for increased data shard allocation to enhance storage node mining efficiency, used during debugging and early testnet phases.

``` yaml
# Workspace is the root directory of all working subdirectories of the node. Please reserve at least 16 GiB of storage space for it.
WorkSpace: "./cacher"
# CacheSize: 17179869184 #default 16GB
# The RPC address of the blockchain where the cache protocol smart contract is deployed, usually the CESS chain
Rpcs: 
  - "wss://xxx.cess.network"
# SecretKey is the key of the node working account (Ethereum wallet account), which is used to initiate a call request to the cache protocol contract (working on EVM). 
# By default, it is not filled in, which means that it does not participate in the CD²N network and only has the most basic data interaction with the gateway.
SecretKey: ""
# Token is the NFT access certificate for nodes to join the CD²N network and will be released in subsequent versions.
Token: ""
# TokenAcc is the holder account(Ethereum wallet account) of the above NFT token.
TokenAcc: ""
# TokenAccSign is an Ethereum account signature, which is the token holder's proof of holding the token. 
# Signature methods and tools will be published in the document.
TokenAccSign: ""
# CD²N cache protocol contract address, which is responsible for node traffic statistics and reward distribution, and works on EVM.
ProtoContract: "0xce078A9098dF68189Cbe7A42FC629A4bDCe7dDD4"
# Local storage nodes configuration file, currently only available for the "Cess Multi-Miner Admin" script.
# The cacher automatically imports the storage node information started by the script through it.
MinerConfigPath: "/opt/cess/mineradm/config.yaml"
# You can manually configure the following connection options to make the cacher serve the specified retriever node:
# By default, it points to the CESS official retriever node. 
# If you register your cacher to the cache protocol contract, 
# it will automatically connect to some publicly available retriever nodes to get more opportunities to get rewards.
Retrievers:
  - Account: "0xb7B43408864aEa0449D8F813380f8ec424F7a775" 
    Endpoint: "http://154.194.34.195:1306" 

# You can also manually import storage nodes through the following configuration. 
# The cacher will automatically check the availability of the storage node and complete other information from the chain.
# StorageNodes:
#   - Account: ""  # CESS account address
#     Endpoint: "" # Http address
```

## Running via Docker

Download the cesslab/cacher image from Docker Hub and launch with:

``` shell
sudo docker run -d --name cacher  \
    -v /opt/cd2n/cacher:/opt/cess  \
    -v /opt/cess/config.yaml:/opt/cess/config.yaml \
    cesslab/cacher:premainnet -c config.yaml run
```
Replace /opt/cd2n/cacher with your host's data directory and `/opt/cess/config.yaml` with your config file path. Configuration updates require container restart.

## Running via Nodeadm

The mineradm program (latest version) will natively support Cacher operations, enabling communication between storage nodes in NAT environments and Retriever nodes for bidirectional data flow between users and the CESS network. By default, Cacher operates in altruistic mode. Configure Cacher nodes through mineradm's default config file at `/opt/cess/mineradm/config.yaml`.