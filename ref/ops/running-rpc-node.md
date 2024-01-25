RPC nodes do not directly participate in block production like consensus nodes. Instead, they are responsible for verifying transactions and facilitating communication between different nodes and between nodes and clients, promoting transaction verification and on-chain information retrieval. There are two ways to run your own RPC node:

1. Start it through the [**cess-nodeadm**](https://github.com/CESSProject/cess-nodeadm)
2. Run [**cess-node**](https://github.com/CESSProject/cess) directly.

## Running an RPC Node through cess-nodeadm

1. Check the latest version of cess-nodeadm
   Latest version of cess-nodeadm: <https://github.com/CESSProject/cess-nodeadm/tags><br/>
   ⚠️ Replace all occurrences of `x.x.x` in the following text with the latest version number. For example, if the latest version is `v0.5.3`, then replace `x.x.x` with `0.5.3`.

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

## Running an RPC Node Directly

1. Install the Rust environment
   Refer to [the official Substrate tutorial](https://docs.substrate.io/install/)

2. Get the latest release version of cess-node
   [Check the latest version of cess-node](https://github.com/CESSProject/cess/tags)

   Taking v0.7.5 as the latest version, download and unzip the **cess-node**:

   ```shell
   wget https://github.com/CESSProject/cess/archive/v0.7.5.tar.gz
   tar -zxvf v0.7.5.tar.gz
   ```

3. Compile **cess-node**

   Enter the cess-node directory:
   ```shell
   cd cess-0.7.5/
   cargo build --release
   ```

4. Start the RPC service
   ```shell
   # For versions up to and including 0.7.5, enter:
   ./target/release/cess-node --base-path 【Your custom database path】 --chain cess-testnet --port 30333 --ws-port 9944 --rpc-port 9933 --unsafe-rpc-external --unsafe-ws-external --name 【Your custom name】 --rpc-cors all --ws-max-connections 2020 --state-pruning archive

   # For versions after 0.7.5, enter:
   ./target/release/cess-node --base-path 【Your custom database path】 --chain cess-testnet --port 30333 --rpc-port 9944 --unsafe-rpc-external --name 【Your custom name】 --rpc-cors all --rpc-max-connections 2020 --state-pruning archive
   ```

   If the node is printing block synchronization logs, it means it's running successfully.

   ⚠️ You need to keep cess-node running at all times; it is recommended to use `screen` or `tmux` commands to run cess-node in the background.
