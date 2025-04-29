RPC nodes do not directly participate in block production like consensus nodes. Instead, they are responsible for verifying transactions and facilitating communication between different nodes and between nodes and clients, promoting transaction verification and on-chain information retrieval. There are two ways to run your own RPC node:

1. Start it through the [**cess-nodeadm**](https://github.com/CESSProject/cess-nodeadm)
2. Run [**cess-node**](https://github.com/CESSProject/cess) directly.

## Through cess-nodeadm

1. Check the latest version of cess-nodeadm
   Latest version of cess-nodeadm: <https://github.com/CESSProject/cess-nodeadm/tags><br/>
   ⚠️ Replace all occurrences of `x.x.x` in the following text with the latest version number. For example, if the latest version is `v0.5.5`, then replace `x.x.x` with `0.5.5`.

2. Check the installed version of cess-nodeadm
   Enter `cess version` in the console to check if the `nodeadm version` is the latest.
   If nodeadm is the latest version, you can skip step 3. If not, proceed to step 3 to install. If you do not see nodeadm version, it means cess-nodeadm is not installed, and you need to proceed to step 3 to install.

3. Download and install the cess-nodeadm
   ```shell
   wget https://github.com/CESSProject/cess-nodeadm/archive/vx.x.x.tar.gz
   tar -xvf vx.x.x.tar.gz
   cd cess-nodeadm-x.x.x/
   ./install.sh
   ```

4. Stop the RPC node service
   Enter the command: `cess stop chain` to stop the running RPC node service.

5. Define script configuration parameters
   ```shell
   cess config set Enter cess node mode from 'authority/storage/watcher' (current: watcher, press enter to skip): watcher
   Enter cess node name (current: cess, press enter to skip): local-chain
   Enter cess chain pruning mode, 'archive' or number (current: null, press enter to skip): archive  #number of blocks saved
   ```

6. Start the RPC node
   ```shell
   cess start chain
   ```

7. Check if the RPC node is synchronizing blocks normally
  ```shell
  docker logs chain
  ```

## Running Directly

1. Environment Setup Requirements
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

2. Get the latest release version of cess-node
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

3. Compile **cess-node**

   Enter the cess-node directory:
   ```shell
   cargo build --release --features only-attestation --features verify-cesealbin
   ```

   ⚠️ Note: The compilation may take approximately 25 minutes on an 8-core machine.

4. Start the RPC service
   ```shell
   ./target/release/cess-node --base-path 【Your custom database path】 --chain cess-testnet --port 【Your custom p2p port】 --rpc-port 【Your custom rpc port】 --prometheus-external --unsafe-rpc-external --name 【Your custom name】 --rpc-cors all --rpc-max-connections 5000 --state-pruning archive --wasm-runtime-overrides ./scripts/wasm_overrides/testnet/

   ```

   You can use the `-h` flag to view more command options.

   ⚠️ **Special note for --wasm-runtime-overrides**: Due to the online upgrade of SGX in the current testnet, it is necessary to specify special wasm files for overriding. For example, if the `spec_100.wasm` file is located at `/opt/wasm/`, the parameter should be filled in as `--wasm-runtime-overrides /opt/wasm`. You only need to specify the directory path, not the file path. Currently, we have placed the spec_100.wasm file in the `/scripts/wasm_overrides/testnet/` path within the cess repository.

   If the node is printing block synchronization logs, it means it's running successfully.

   ⚠️ You need to keep cess-node running at all times; it is recommended to use `screen` or `tmux` commands to run cess-node in the background.
