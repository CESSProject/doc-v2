RPC nodes do not directly participate in block production like consensus nodes. Instead, they are responsible for verifying transactions and facilitating communication between different nodes and between nodes and clients, promoting transaction verification and on-chain information retrieval.

## 1. Run with cess-nodeadm

1.1 Check the latest version of cess-nodeadm
   Latest version of cess-nodeadm: <https://github.com/CESSProject/cess-nodeadm/tags><br/>
   ⚠️ Replace all occurrences of `x.x.x` in the following text with the latest version number. For example, if the latest version is `v0.6.1`, then replace `x.x.x` with `0.6.1`.

1.2 Check the installed version of cess-nodeadm
   Enter `cess version` in the console to check if the `nodeadm version` is the latest.
   If nodeadm is the latest version, you can skip step 3. If not, proceed to step 3 to install. If you do not see nodeadm version, it means cess-nodeadm is not installed, and you need to proceed to step 3 to install.

1.3 Download and install the cess-nodeadm
   ```shell
   wget https://github.com/CESSProject/cess-nodeadm/archive/vx.x.x.tar.gz
   tar -xvf vx.x.x.tar.gz
   cd cess-nodeadm-x.x.x/
   ./install.sh
   ```

1.4 Stop the RPC node service
   Enter the command: `cess stop chain` to stop the running RPC node service.

1.5 Define script configuration parameters

**The archive mode saves all blocks, which is suitable for full node operation, otherwise, you can set the number of blocks to be saved**
   
   ```shell
   Enter cess node mode from 'tee/storage/validator/rpcnode' (current: rpcnode, press enter to skip): rpcnode
   Enter cess node name (current: cess, press enter to skip): local-chain
   Enter cess chain pruning mode, 'archive' or number (current: archive, press enter to skip): archive  #number of blocks saved
   ```

1.6 Start the RPC node
   ```shell
   cess start chain
   ```

1.7 Check if the RPC node is synchronizing blocks normally
  ```shell
  docker logs chain
  ```

{% hint style="info" %}
An RPC node will also be started automatically when the user runs the storage node using `nodeadm` or `mineradm`, unless an external chain is specified.
{% endhint %}

## 2. Run with source code

2.1 Environment Setup Requirements
   - OS required: Ubuntu 20+
   - Rust install: 
     ```shell
     curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
     ```
   - Dependencies install: 
     ```shell
     apt update && apt install -y gcc llvm clang make git libssl-dev pkg-config wget unzip
     ```
   - ProtoBuf install (run as root): 
     ```shell
     mkdir pb && cd pb \
       && wget --show-progress -q https://github.com/protocolbuffers/protobuf/releases/download/v25.2/protoc-25.2-linux-x86_64.zip \
       && unzip protoc-25.2-linux-x86_64.zip \
       && cp bin/protoc /usr/local/bin/protoc \
       && chmod +x /usr/local/bin/protoc \
       && cp -r include/* /usr/local/include \
       && rm -rf ./* \
       && protoc --version
     ```

2.2 Get the latest release version of cess-node
   [Check the latest version of cess-node](https://github.com/CESSProject/cess/tags)

   You can get the latest version using one of the following methods:

   **Method 1**: Download and unzip the release (e.g., cess-0.7.9-venus as example):
   ```shell
   wget https://github.com/CESSProject/cess/archive/refs/tags/cess-v0.7.9-venus.tar.gz
   tar -zxvf cess-cess-v0.7.9-venus.tar.gz
   ```

   **Method 2**: Clone the repository with the latest tag:
   ```shell
   git clone https://github.com/CESSProject/cess.git
   cd cess
   git checkout $(git describe --tags $(git rev-list --tags --max-count=1))
   ```

2.3 Compile **cess-node**

   Enter the cess-node directory:
   ```shell
   cargo build --release --features only-attestation --features verify-cesealbin
   ```

   ⚠️ Note: The compilation may take approximately 25 minutes on an 8-core machine.

2.4 Start the RPC service
   ```shell
   ./target/release/cess-node --base-path 【Your custom database path】 --chain cess-testnet --port 【Your custom p2p port】 --rpc-port 【Your custom rpc port】 --prometheus-external --unsafe-rpc-external --name 【Your custom name】 --rpc-cors all --rpc-max-connections 5000 --state-pruning archive --wasm-runtime-overrides ./scripts/wasm_overrides/testnet/
   ```

   Use the `-h` flag to view more command options.

   ⚠️ **Special note for --wasm-runtime-overrides**: Due to the online upgrade of SGX in the current testnet, it is necessary to specify special wasm files for overriding. For example, if the `spec_100.wasm` file is located at `/opt/wasm/`, the parameter should be filled in as `--wasm-runtime-overrides /opt/wasm`. You only need to specify the directory path, not the file path. Currently, we have placed the spec_100.wasm file in the `/scripts/wasm_overrides/testnet/` path within the cess repository.

   If the node is printing block synchronization logs, it means it's running successfully.

   ⚠️ It is recommended to use `systemd`, `screen` or `tmux` commands to run cess-node(RPC) if you want to keep cess-node running.

## 3. Run with Container

3.1 Environment Setup Requirements
     ```shell
     curl -fsSL https://get.docker.com | bash
     docker --version
     docker pull cesslab/cess-chain:testnet
     ```

3.2 Running Command

   **Make sure that port 30336 and 9944 are not occupied by other processes.**

   ```bash
   mkdir -p /opt/cess/testnet-rpc-data

   docker run -d \
   --name testnet-rpc \
   -v /opt/cess/testnet-rpc-data:/opt/cess/data \
   -p 30336:30336 \
   -p 9944:9944 \
   cesslab/cess-chain:testnet \
   --base-path /opt/cess/data \
   --chain cess-testnet \
   --port 30336 \
   --rpc-port 9944 \
   --rpc-external \
   --execution WASM \
   --wasm-execution compiled \
   --in-peers 75 \
   --out-peers 75 \
   --state-pruning archive \
   --rpc-max-connections 65535 \
   --rpc-cors all \
   --prometheus-external \
   --wasm-runtime-overrides /opt/cess/wasms
   ```

   **Execute `docker run -it --rm --entrypoint /opt/cess/cess-node cesslab/cess-chain:testnet --help` to get more information about the command options.**


3.3 Check if the RPC node is synchronizing blocks normally

   ```bash
      docker logs testnet-rpc
   ```

   The rpc node log is down below and start to synchronize blocks.

   ```text
      2025-04-30 09:47:13 CESS Node
      2025-04-30 09:47:13 ✌️  version 0.9.0-9936f4e
      2025-04-30 09:47:13 ❤️  by CESS LAB, 2017-2025
      2025-04-30 09:47:13 📋 Chain specification: cess-testnet
      2025-04-30 09:47:13 🏷  Node name: better-ink-0467
      2025-04-30 09:47:13 👤 Role: FULL
      2025-04-30 09:47:13 💾 Database: RocksDb at /opt/cess/data/chains/cess-testnet/db/full
      2025-04-30 09:47:17 Found wasm override. version=cess-node-100 (cess-node-0.tx1.au1) file=/opt/cess/wasms/spec_100.wasm
      2025-04-30 09:47:17 Found wasm override. version=cess-node-100 (cess-node-0.tx1.au1) file=/opt/cess/wasms/spec_100.wasm
      2025-04-30 09:47:20 Using default protocol ID "sup" because none is configured in the chain specs
      2025-04-30 09:47:20 🏷  Local node identity is: 12D3KooWJJbr7xMCByzzHCUD8NXz52JDRC7pcXdf6otopvZCAreG
      2025-04-30 09:47:20 Running libp2p network backend
      2025-04-30 09:47:20 💻 Operating system: linux
      2025-04-30 09:47:20 💻 CPU architecture: x86_64
      2025-04-30 09:47:20 💻 Target environment: gnu
      2025-04-30 09:47:20 💻 CPU: Intel(R) Xeon(R) Gold 6148 CPU @ 2.40GHz
      2025-04-30 09:47:20 💻 CPU cores: 40
      2025-04-30 09:47:20 💻 Memory: 257412MB
      2025-04-30 09:47:20 💻 Kernel: 6.8.0-51-generic
      2025-04-30 09:47:20 💻 Linux distribution: Ubuntu 20.04.6 LTS
      2025-04-30 09:47:20 💻 Virtual machine: no
      2025-04-30 09:47:20 📦 Highest known block at #2370
      2025-04-30 09:47:20 Running JSON-RPC server: addr=0.0.0.0:9944,[::]:36421
      2025-04-30 09:47:20 🏁 CPU single core score: 760.78 MiBs, parallelism score: 734.78 MiBs with expected cores: 8
      2025-04-30 09:47:20 🏁 Memory score: 4.82 GiBs
      2025-04-30 09:47:20 🏁 Disk score (seq. writes): 768.54 MiBs
      2025-04-30 09:47:20 🏁 Disk score (rand. writes): 337.66 MiBs
      2025-04-30 09:47:20 〽️ Prometheus exporter started at 0.0.0.0:9615
      2025-04-30 09:47:20 discovered: 12D3KooWEpnboX55ceLkf2wqUQ6LBujEReiCqmvj5SLLdoJJP3oQ /ip4/172.17.0.1/tcp/30336/ws
      2025-04-30 09:47:20 🔍 Discovered new external address for our node: /ip4/154.194.34.206/tcp/30336/ws/p2p/12D3KooWJJbr7xMCByzzHCUD8NXz52JDRC7pcXdf6otopvZCAreG
      2025-04-30 09:47:23 [#2469] 🗳  Starting phase Off, round 2.
      2025-04-30 09:47:23 [2469] 💸 new validator set of size 5 has been processed for era 1
      2025-04-30 09:47:25 ⚙️  Syncing, target=#3711019 (5 peers), best: #2939 (0x87db…8c38), finalized #2936 (0xa33a…c890), ⬇ 757.8kiB/s ⬆ 5.7kiB/s
   ```
