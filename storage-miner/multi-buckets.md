# Architecture

Install multi-buckets container can be illustrated as below:
- watchTower: When there is a difference between the local bucket image and the official bucket image, watchtower will automatically pull the new official image, create a new storage node, and then delete the old storage node.
- bucket: A storage node. Multiple storage nodes communicate with each other via P2P. The ports configured in the example config.yaml are: 15001, 15002.
- chain: A chain node. Storage nodes query block information through the chain node's 9944 port by default; chain nodes synchronize data among themselves through the default port: 30336.

![Multi-bucket Architecture](../assets/storage-miner/multi-buckets/multibucket.png)

# System requirements

Minimum Configuration Requirements:

| Resource  | Specification |
| ------------- | ------------- |
| Recommended OS | Linux 64-bit Intel / AMD |
| # of CPU Cores | ≥ 4 |
| Memory | ≥ 8 GB |
| Bandwidth | ≥ 5 Mbps |
| Public Network IP | required |
| Linux Kernel Version | 5.11 or higher |

Each storage node requires at least 4GB of RAM and 1 logic core, and the chain node requires at least 2GB of RAM and 1 logic core.

At least 10GB of RAM and 3 logic cores if running 2 storage nodes and 1 chain node at the same time

# Method 1: Run multi-buckets containers with admin client

## Storage environment requirements

Installation operation has certain requirements on the storage environment in the current host, 
and different configurations are required based on the disk configuration.

### Multiple Disks

As shown in the figure below, where `/dev/sda` is the system disk, `/dev/sdb`, `/dev/sdc` are the data disks, users can directly partition and create file systems on the data disks, 
and finally mount the file systems to the working directory of the storage node.

![Multi Disk](../assets/storage-miner/multi-buckets/multi-disk-env.png)

```bash
fdisk /dev/sdb

# 2048: The starting sector of a new disk is usually set to 2048. This ensures that the partition boundaries are aligned with the physical sectors of the hard disk.
# the value after default: The default is the maximum sector value, which partitions the entire disk.

Enter and press Enter:
n
p
1
2048
the value after default
w

# create filesystem in /dev/vdb
sudo mkfs.ext4 /dev/sdb

Proceed anyway? (y,N) y

# create a diskPath of a storage node
sudo mkdir /cess_storage1

# mount filesystem
sudo mount /dev/sdb /cess_storage1

# auto mount when your reboot your server
sudo cp /etc/fstab /etc/fstab.bak
sudo sh -c "echo `blkid /dev/sdb | awk '{print $2}' | sed 's/\"//g'` /cess_storage1 ext4 defaults 0 0 >> /etc/fstab"
```

Repeat the above steps to partition `/dev/sdc` and create a filesystem, then mount it to the file directory: `/cess_storage2`

{% hint style="warning" %}
In the case where a disk is divided into multiple partitions, when the disk is damaged, all storage nodes that use its partitions for work will be affected.
{% endhint %}

### Single Disk
This procedure is suitable for environments with only one system disk.

#### Scene 1
As shown in the following example, if there is only one 50GB system disk, 
the `Last sector value` of partition `/dev/sda3` of disk `/dev/sda` is already at its maximum value (50GB disk can not be partitioned any more).
```bash
[root@cess ~]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda    253:0    0   50G  0 disk 
├─sda1 253:1    0    2M  0 part 
├─sda2 253:2    0  200M  0 part /boot/efi
└─sda3 253:3    0 49.8G  0 part /
```
As shown above, the current system kernel is using this partition, so it can not modify the partition to build the running environment required for multibucket.

If the partition does not take up the entire disk and there is still storage space available for partitioning, you can configure the partition by referring to the configuration method of **Multiple Disks**.(In this situation, the running of multibucket will depend on this single disk.)


#### Scene 2
As shown in the figure below, the current environment has only one `/dev/nvme0n1` system disk with about 1.8T of storage space, which is partitioned three times, including `/dev/nvme0n1p1`, `/dev/nvme0n1p2` and `/dev/nvme0n1p3`.

The current system relies on the virtual logical disk `/dev/ubuntu-vg/ubuntu-lv` created in the third partition `/dev/nvme0n1p3`. Since this virtual logical disk occupies only 100GB of storage space, you can configure a multibucket environment by using `lvm` to create multiple virtual logical volumes on the remaining space.

![Single Disk](../assets/storage-miner/multi-buckets/single-disk-env.png)

```bash
# use command: vgs to show current volume group, and find that the current volume group name is: ubuntu-vg, VFree displays the remaining storage space of the current volume group.
$ vgs
cess@cess:/home/cess# vgs
  VG        #PV #LV #SN Attr   VSize   VFree
  ubuntu-vg   1   1   0 wz--n- <1.82t  1.7T

# use command: lvcreate to create a 100GB logic volume named cess_storage from volume group: ubuntu-vg
$ sudo lvcreate -L 100g -n cess_storage ubuntu-vg -y
# use command: lvcreate to create logic volume named cess_storage from all remaining space of volume group: ubuntu-vg
# sudo lvcreate -l 100%FREE -n cess_storage ubuntu-vg -y

# use command: lvdisplay to display logic volume your have created, name: cess_storage, path: /dev/ubuntu-vg/cess_storage
$ sudo lvdisplay
root@cess:/home/cess# lvdisplay
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv
  LV Name                ubuntu-lv
  VG Name                ubuntu-vg
  LV UUID                zxJiPj-Anon-CG3r-XEIJ-Nydi-xxxx-U6oWqW
  LV Size                100.00 GiB
   
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/cess_storage
  LV Name                cess_storage
  VG Name                ubuntu-vg
  LV UUID                33Z2eL-AVma-oV4V-1vnE-G3YC-xxxx-wtzxHs
  LV Size                <1.72 TiB

# create filesystem in /dev/ubuntu-vg/cess_storage
$ sudo mkfs.ext4 /dev/ubuntu-vg/cess_storage

# create a diskPath of a storage node
sudo mkdir /cess

# mount filesystem
sudo mount /dev/ubuntu-vg/cess_storage /cess

# auto mount when your reboot your server
sudo cp /etc/fstab /etc/fstab.bak
# modify <lv path>, <diskPath>, <filesystem type>
sudo sh -c "echo `blkid /dev/ubuntu-vg/cess_storage | awk '{print $2}' | sed 's/\"//g'` /cess ext4 defaults 0 0 >> /etc/fstab"
```

{% hint style="warning" %}
Users can create multiple logic volumes on a single disk by lvm, and mount multiple logic volumes on different diskPaths, but when the disk is damaged, all storage nodes relying on lvm will be down!
{% endhint %}


## 1. Download and install cess-multibucket-admin client

```bash
sudo wget https://github.com/CESSProject/cess-multibucket-admin/archive/latest.tar.gz
sudo tar -xvf latest.tar.gz
cd cess-multibucket-admin-latest
sudo bash ./install.sh
```

## 2. Customize your own configuration

{% hint style="info" %}
After executing the above installation command, customize your own config file at: `/opt/cess/multibucket-admin/config.yaml`.
{% endhint %}

- UseSpace: Storage capacity of the storage node, measured in GB.
- UseCpu: Number of logical cores used by the storage node.
- port: Storage node use that port to communicat with each other，the port of each storage node must be different and not occupied by other process
- diskPath: Absolute system path where the storage node run, requiring a file system to be mounted at this path.  
- earningsAcc: Used to receive mining rewards. [Get earningsAcc and mnemonic](https://docs.cess.cloud/core/storage-miner/running#prepare-cess-accounts)
- stakingAcc: Used to pay for staking TCESS. 4000 TCESS is required for providing 1T of storage space.
- mnemonic: Account mnemonic, consisting of 12 words, with each storage node requiring a different mnemonic.
- chainWsUrl: By default, the local RPC node will be used for  data synchronization. The priority of `buckets[].chainWsUrl` is higher than `node.chainWsUrl`.
- backupChainWsUrls: Backup RPC nodes that can be official RPC nodes or other RPC nodes you know. The priority of `buckets[].backupChainWsUrls` is higher than `node.backupChainWsUrls`.

{% hint style="warning" %}
Your can run multibucket in a single disk by lvm, then mount each lv in different diskPath, but when your single disk can not work, all storage nodes depends on this single disk will be down !
{% endhint %}


   ```yaml
   ## node configurations template
   node:
      ## the mode of node: multibucket
      mode: "multibucket"
      ## the profile of node: devnet/testnet/mainnet
      profile: "testnet"
      # default chain url for bucket, can be overwritten in buckets[] as below
      chainWsUrl: "ws://127.0.0.1:9944/"
      # default backup chain urls for bucket, can be overwritten in buckets[] as below
      backupChainWsUrls: ["wss://testnet-rpc0.cess.cloud/ws/", "wss://testnet-rpc1.cess.cloud/ws/", "wss://testnet-rpc2.cess.cloud/ws/", "wss://testnet-rpc3.cess.cloud/ws/"]

   ## chain configurations
   ## set option: '--skip-chain' or '-s' to skip installing chain (cess-multibucket-admin install --skip-chain)
   ## if set option: --skip-chain, please set official chain in buckets[].chainWsUrl or others chains you know
   chain:
      ## the name of chain node
      name: "cess"
      ## the port of chain node
      port: 30336
      ## listen rpc service at port 9944
      rpcPort: 9944

   ## bucket configurations  (multi storage miner mode)
   buckets:
      - name: "bucket_1"
        # P2P communication port
        port: 15001
        # Maximum space used, the unit is GiB
        UseSpace: 1000
        # Number of cpu's used, 0 means use all
        UseCpu: 2
        # earnings account
        earningsAcc: "cXxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        # Staking account
        # If you fill in the staking account, the staking will be paid by the staking account,
        # otherwise the staking will be paid by the earningsAcc.
        stakingAcc: "cXxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        # Signature account mnemonic
        # each bucket's mnemonic should be different
        mnemonic: "aaaaa bbbbb ccccc ddddd eeeee fffff ggggg hhhhh iiiii jjjjj kkkkk lllll"
        # a directory mount with filesystem
        diskPath: "/mnt/cess_storage1"
        # The rpc endpoint of the chain
        # `official chain: wss://testnet-rpc0.cess.cloud/ws/ wss://testnet-rpc1.cess.cloud/ws/ wss://testnet-rpc2.cess.cloud/ws/` "wss://testnet-rpc3.cess.cloud/ws/"
        chainWsUrl: "ws://127.0.0.1:9944/"
        backupChainWsUrls: []
        # Priority tee list address
        TeeList:
           - "127.0.0.1:8080"
           - "127.0.0.1:8081"
        # Bootstrap Nodes
        Boot: "_dnsaddr.boot-bucket-testnet.cess.cloud"
        
      - name: "bucket_2"
        # P2P communication port
        port: 15002
        # Maximum space used, the unit is GiB
        UseSpace: 1000
        # Number of cpu's used
        UseCpu: 2
        # earnings account
        earningsAcc: "cXxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        # Staking account
        # If you fill in the staking account, the staking will be paid by the staking account,
        # otherwise the staking will be paid by the earningsAcc.
        stakingAcc: "cXxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        # Signature account mnemonic
        # each bucket's mnemonic should be different
        mnemonic: "lllll kkkkk jjjjj iiiii hhhhh ggggg fffff eeeee ddddd ccccc bbbbb aaaaa"
        # a directory mount with filesystem
        diskPath: "/mnt/cess_storage2"
        # The rpc endpoint of the chain
        # `official chain: wss://testnet-rpc0.cess.cloud/ws/ wss://testnet-rpc1.cess.cloud/ws/ wss://testnet-rpc2.cess.cloud/ws/` "wss://testnet-rpc3.cess.cloud/ws/"
        chainWsUrl: "ws://127.0.0.1:9944/"
        backupChainWsUrls: [ ]
        # Priority tee list address
        TeeList:
           - "127.0.0.1:8080"
           - "127.0.0.1:8081"
        # Bootstrap Nodes
        Boot: "_dnsaddr.boot-bucket-testnet.cess.cloud"
   ```

## 3. Generate configuration
   

The following command will generate `config.yaml` for each storage node and `docker-compose.yaml` based on the file located at: `/opt/cess/multibucket-admin/config.yaml`.

   ```bash
   sudo cess-multibucket-admin config generate
   ```

- Generate each bucket configuration at `$diskPath/bucket/config.yaml`. For example, bucket_1's configuration generate at: `/mnt/cess_storage1/bucket/config.yaml`
- Generate docker-compose.yaml at `/opt/cess/multibucket-admin/build/docker-compose.yaml`

{% hint style="warning" %}
Unless the path to the configuration file has been customized, `/opt/cess/multibucket-admin/config.yaml` will be used.
{% endhint %}

## 4. Installation

### Install all services
Install watchTower, rpc, and multi-buckets services

  ```bash
  sudo cess-multibucket-admin install
  ```

### Skip install rpcnode
If an official RPC node or other known RPC node is configured in the configuration file, you can skip starting a local RPC node with `-skip-chain`.

  ```bash
  sudo cess-multibucket-admin install --skip-chain
  ```

## 5. Common Operations

**Stop one container**, such as execute `sudo cess-multibucket-admin stop bucket_1` to stop `bucket_1`
```bash
  sudo cess-multibucket-admin stop $1
```

**Stop all containers**
```bash
  sudo cess-multibucket-admin stop
```

**Stop and remove all containers**
```bash
  sudo cess-multibucket-admin down
```

**Restart all services**
```bash
  sudo cess-multibucket-admin restart
```

**Restart a service**, such as execute `sudo cess-multibucket-admin reload bucket_1` to restart `bucket_1`
```bash
  sudo cess-multibucket-admin restart $1
```

**Get version information** 
```bash
  sudo cess-multibucket-admin version
```

**Check services status**
```bash
  sudo cess-multibucket-admin status
```

**Pull images**
```bash
  sudo cess-multibucket-admin pullimg
```

**Check disk usage** 
```bash
  sudo cess-multibucket-admin tools space-info
```

**View Bucket Status**

Please wait hours to wait data sync in storage node
```bash
  sudo cess bucket stat
```

**Increase Miner Staking**
```bash
  sudo cess-multibucket-admin buckets increase staking <deposit amount>
```

**Withdraw Miner Staking**

After your node **has exited CESS Network** (see below), run
```bash
  sudo cess-multibucket-admin buckets withdraw
```

**Query Reward Information**
```bash
  sudo cess-multibucket-admin buckets reward
```

**Claim Reward**
```bash
  sudo cess-multibucket-admin buckets claim
```

**Update Earnings Account**
```bash
  sudo cess-multibucket-admin buckets update earnings [earnings account]
```

**Exit CESS network**
```bash
  sudo cess-multibucket-admin buckets exit
```

**Remove all service**

{% hint style="warning" %}
Do not perform this operation unless the CESS network has been redeployed and it is confirmed that the data can be cleared.
{% endhint %}
```bash
  sudo cess-multibucket-admin purge
```

# Method 2: Run multi-buckets containers manually

## 1. Prerequisites

### I. PRC Node
To set up a multi-bucket environment, you must have an RPC node. You can either set it up locally or utilize the nodes officially provided by CESS. 

- To set up an RPC node locally please follow [Running RPC Node](../ref/ops/running-rpc-node.md)
- CESS officially hosted RCP nodes
  
   ```
   wss://testnet-rpc0.cess.cloud/ws/
   wss://testnet-rpc1.cess.cloud/ws/
   wss://testnet-rpc2.cess.cloud/ws/
   ```

### II. Install Docker and Docker Compose

Using ubuntu (officially recommended storage node operating system) as an example to install docker.

Also refer to the Docker [installation documentation](https://docs.docker.com/engine/install/).

1. Update system package list

   ```bash
   sudo apt update
   ```

2. Install necessary dependencies:

   ```bash
   sudo apt install apt-transport-https ca-certificates curl    software-properties-common
   ```

3. Add Docker GPG key：

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. Set up Docker repository：

   ```bash
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. Update package index and install Docker engine：

   ```bash
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

6. Add the current user to the docker group so that you do not need to use the sudo command to run docker. It will take effect after restarting:

   ```bash
   sudo usermod -aG docker $USER
   ```

   
## 2. Configure Multi-bucket Storage Node

1. Create a working directory for each of the storage nodes. For instance, in a two instance of storage node configuration, if you would like to allocate two separate disks to each of the nodes you can create two separate directories and mount the desired disk on each of the directories. Here we have mounted disk0 to `/mnt/disk0` and disk1 to `/mnt/disk1`. You may modify the directory name as you like.

   ```bash
   cd /mnt/disk0/
   mkdir bucket storage
   cd /mnt/disk1/
   mkdir bucket storage
   ```

   Here, the `bucket` directory is used to store storage node configuration files, and the `storage` directory is used as the working directory for storage node operation.

2. Create a 'config.yaml' file in the bucket directory of each storage node and paste the following content:

   ```yaml
   # The rpc endpoint of the chain node
   Rpc:
     # - "ws://127.0.0.1:9944/"
     - wss://testnet-rpc0.cess.cloud/ws/
     - wss://testnet-rpc1.cess.cloud/ws/
     - wss://testnet-rpc2.cess.cloud/ws/

   # Bootstrap Nodes
   Boot:
     - "_dnsaddr.boot-bucket-testnet.cess.cloud"
   # Signature account mnemonic
   Mnemonic: "xxx xxx ... xxx"
   # Staking account
   # If you fill in the staking account, the staking will be paid by the staking account,
   # otherwise the staking will be paid by the signature account.
   StakingAcc: "cXxxx...xxx"
   # earnings account
   EarningsAcc: cXxxx...xxx
   # Service workspace
   Workspace: "/opt/bucket-disk"
   # P2P communication port
   Port: 4001
   # Maximum space used, the unit is GiB
   UseSpace: 2000
   # Number of cpu's used, 0 means use all
   UseCpu: 4
   # Priority tee list address
   TeeList:
   #  - "127.0.0.1:8080"
   #  - "127.0.0.1:8081"
   ```

   Please note that when configuring RPC, the first one `(ws://127.0.0.1:9944/)` is the local RPC node address. Use this address if you have chosen to set up your own RPC node. If you haven't set up your own RPC node, please use the officially provided PRC addresses.

   ⚠️  Make sure to update `Mnemonic`, `StakingAcc`, `EarningsAcc`, and `UseCpu`.

3. The directory structure for each of the storage nodes should be as shown in the figure below：

   ![folder structure](../assets/storage-miner/multi-buckets/folder.png)

## 3. Configure and start the storage node container

Please create the `docker-compose.yaml` file with the following content to start storage node containers in batches, you can place this file anywhere accessible.

```yaml
version: '3'
name: cess-storage
services:
  bucket_0: # services name
    image: 'cesslab/cess-bucket:testnet'
    network_mode: host
    restart: always
    volumes: # Mapping of host disk to container
      - '/mnt/disk0/bucket:/opt/bucket' # Node configuration directory
      - '/mnt/disk0/storage/:/opt/bucket-disk' # Node working directory
    command:
      - run
      - '-c'
      - /opt/bucket/config.yaml # Make sure that config.yaml matches with the configuration file we created above.
    logging:
      driver: json-file
      options:
        max-size: 500m
    container_name: bucket0 # Container name
  bucket_1:
    image: 'cesslab/cess-bucket:testnet'
    network_mode: host
    restart: always
    volumes:
      - '/mnt/disk1/bucket:/opt/bucket' # Node configuration directory
      - '/mnt/disk1/storage/:/opt/bucket-disk' # Node working directory,
    command:
      - run
      - '-c'
      - /opt/bucket/config.yaml # Make sure that config.yaml matches with the configuration file we created above.
    logging:
      driver: json-file
      options:
        max-size: 500m
    container_name: bucket1
  watchtower: # Only needs to be run once
    image: containrrr/watchtower
    container_name: watchtower
    network_mode: host
    restart: always
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock'
    command:
      - '--cleanup'
      - '--interval'
      - '300'
      - '--enable-lifecycle-hooks'
      - chain
      - bucket
    logging:
      driver: json-file
      options:
        max-size: 100m
        max-file: '7'
```

In the above file, set up a storage node service for each running storage node container. Depending on the number of storage nodes you are configuring the number of services also needs to be added. In our example, we are setting up two services `bucket_0` and `bucket_1`. In each service, it is important to focus on configuring the service name, container name, and directory mapping from the host to the container; such as in `bucket_0`, the directory mapping configuration is as follows：

```yaml
volumes: # Mapping of host disk to container
  - '/mnt/disk0/bucket:/opt/bucket' # Node configuration directory
  - '/mnt/disk0/storage/:/opt/bucket-disk' # Node working directory
```

Among them, `/mnt/disk0/bucket/` is the previously created directory to store the configuration file `config.yaml`, and `/mnt/disk0/storage/` is the working directory for storage node 0.

The `watchtower` service is used to monitor the status of each storage node container and automatically update the latest image for the container. Each server only needs to configure one such service.

After configuring the file, run the following command to start the storage node containers.

```bash
docker compose up -d
```

You can check the running status of your docker containers by executing the following command 

```bash
docker ps -a
``` 

To check the logs for each of the storage nodes execute

```bash
# Replace <STORAGE_CONTAINER_NAME> with bucket0, bucket1
docker logs <STORAGE_CONTAINER_NAME> 
```

To check the status of the storage miner execute
```bash
# Replace <STORAGE_CONTAINER_NAME> with bucket0, bucket1
docker exec <STORAGE_CONTAINER_NAME> cess-bucket --config /opt/bucket/config.yaml stat
```

Similarly, you can execute all the bucket related commands 
```bash
docker exec <STORAGE_CONTAINER_NAME> cess-bucket --config /opt/bucket/config.yaml <COMMAND>
```
