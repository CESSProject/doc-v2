# Architecture

Install multi-miners container can be illustrated as below:
- watchTower: When there is a difference between the local miner image and the official miner image, watchtower will automatically pull the new official image, create a new storage node, and then delete the old storage node.
- miner: A storage node. Multiple storage nodes communicate with each other via P2P. The ports configured in the example config.yaml are: 15001, 15002.
- chain: A chain node. Storage nodes query block information through the chain node's 9944 port by default; chain nodes synchronize data among themselves through the default port: 30336.

![Multi-miner Architecture](../assets/storage-miner/multi-miner/multi-miner-arch.png)

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

Each storage node requires at least 4GB of RAM and 1 processor, and the chain node requires at least 2GB of RAM and 1 processor.

At least 10GB of RAM and 3 processors if running 2 miners and 1 chain node at the same time

# Method 1: Run multi-miners containers with admin client

## Storage environment requirements

Installation operation has certain requirements on the storage environment in the current host, 
and different configurations are required based on the disk configuration.

### Multiple Disks

As shown in the figure below, where `/dev/sda` is the system disk, `/dev/sdb` and `/dev/sdc` is the data disk, users can directly partition and create file systems on the data disks, 
and finally mount the file systems to the working directory of the miner.

![Multi Disk](../assets/storage-miner/multi-miner/multi-disk-env.png)

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
sudo mkdir /mnt/cess_storage1

# mount filesystem
sudo mount /dev/sdb /mnt/cess_storage1

# auto mount when your reboot your server
sudo cp /etc/fstab /etc/fstab.bak

# modify <disk: /dev/sdb> <mount path: /mnt/cess_storage1>
sudo sh -c "echo `blkid /dev/sdb | awk '{print $2}' | sed 's/\"//g'` /mnt/cess_storage1 ext4 defaults 0 0 >> /etc/fstab"
```

Repeat the above steps to partition `/dev/sdc` and create a filesystem, then mount it to the file directory: `/mnt/cess_storage2`

{% hint style="warning" %}
In the case where a disk is divided into many partitions, when the disk is damaged, all storage nodes that use its partitions for work will be affected.
{% endhint %}

### Single Disk
This procedure is suitable for environments with only one system disk.

#### Scene 1
As shown in the following example, if there is only one 50GB system disk, 
the `Last sector value` of partition `/dev/sda3` of disk `/dev/sda` is already at its maximum value (50GB disk can not be partitioned anymore).
```bash
[cess@cess ~]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda    253:0    0   50G  0 disk 
├─sda1 253:1    0    2M  0 part 
├─sda2 253:2    0  200M  0 part /boot/efi
└─sda3 253:3    0 49.8G  0 part /
```
As shown above, the current system kernel is using this partition, so it can not modify the partition to build the running environment required for multi-miner.

If the partition does not take up the entire disk and there is still storage space available for partitioning, you can configure the partition by referring to the configuration method of **Multiple Disks**.(In this situation, the running of multi-miner will depend on this single disk)


#### Scene 2
As shown in the figure below, the current environment has only one `/dev/nvme0n1` system disk with about 1.8T of storage space, which is partitioned three times, including `/dev/nvme0n1p1`, `/dev/nvme0n1p2` and `/dev/nvme0n1p3`.

The current system relies on the virtual logical disk `/dev/ubuntu-vg/ubuntu-lv` created in the third partition `/dev/nvme0n1p3`. Since this virtual logical disk occupies only 100GB of storage space, you can configure a multi-miner environment by using `lvm` to create multiple virtual logical volumes on the remaining space.

![Single Disk](../assets/storage-miner/multi-miner/single-disk-env.png)

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
cess@cess:/home/cess# lvdisplay
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
Users can create multiple logic volumes on a single disk by lvm, and mount multiple logic volumes on different diskPaths, but when the disk is damaged, all storage nodes relying on lvm will be affected!
{% endhint %}


## 1. Download and install cess-multi-miner client

```bash
sudo wget https://github.com/CESSProject/cess-multi-miner/archive/latest.tar.gz
sudo tar -xvf latest.tar.gz
cd cess-multi-miner-latest
sudo bash ./install.sh
```

## 2. Customize your own configuration

{% hint style="info" %}
After executing the above installation command, customize your own config file at: `/opt/cess/mineradm/config.yaml`.
{% endhint %}

- UseSpace: Storage capacity of the storage node, measured in GB.
- UseCpu: Number of logical cores used by the storage node.
- port: Storage node use that port to communicat with each other，the port of each storage node must be different and not occupied by other process
- diskPath: Absolute system path where the storage node run, requiring a file system to be mounted at this path.  
- earningsAcc: Used to receive mining rewards. [Get earningsAcc and mnemonic](https://docs.cess.cloud/core/storage-miner/running#prepare-cess-accounts)
- stakingAcc: Used to pay for staking TCESS. 4000 TCESS is required for providing 1T of storage space. Delete in config.yaml can stake by earningsAcc.
- mnemonic: Account mnemonic, consisting of 12 words, with each storage node requiring a different mnemonic.
- chainWsUrl: By default, the local RPC node will be used for data synchronization. The priority of `miners[].chainWsUrl` is higher than `node.chainWsUrl`.
- backupChainWsUrls: Backup RPC nodes that can be official RPC nodes or other RPC nodes you know. The priority of `miners[].backupChainWsUrls` is higher than `node.backupChainWsUrls`.

{% hint style="warning" %}
Your can run multi-miner in a single disk by lvm, then mount each lv in different diskPath, but when your single disk can not work, all storage nodes depends on this single disk will be affected!
{% endhint %}


   ```yaml
   ## node configurations template
   node:
      ## the mode of node: multi-miner
      mode: "multi-miner"
      ## the profile of node: devnet/testnet/mainnet
      profile: "testnet"
      # default chain url for miner, can be overwritten in miners[] as below
      chainWsUrl: "ws://127.0.0.1:9944/"
      # default backup chain urls for miner, can be overwritten in miners[] as below
      backupChainWsUrls: ["wss://testnet-rpc0.cess.cloud/ws/", "wss://testnet-rpc1.cess.cloud/ws/", "wss://testnet-rpc2.cess.cloud/ws/", "wss://testnet-rpc3.cess.cloud/ws/"]

   ## chain configurations
   ## set option: '--skip-chain' or '-s' to skip installing chain (mineradm install --skip-chain)
   ## if set option: --skip-chain, please set official chain in miners[].chainWsUrl or others chains you know
   chain:
      ## the name of chain node
      name: "cess"
      ## the port of chain node
      port: 30336
      ## listen rpc service at port 9944
      rpcPort: 9944

   ## miner configurations  (multi storage miner mode)
   miners:
      - name: "miner1"
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
        # each miner's mnemonic should be different
        mnemonic: "aaaaa bbbbb ccccc ddddd eeeee fffff ggggg hhhhh iiiii jjjjj kkkkk lllll"
        # miner work at this path
        diskPath: "/mnt/cess_storage1"
        # The rpc endpoint of the chain
        # `official chain: wss://testnet-rpc0.cess.cloud/ws/ wss://testnet-rpc1.cess.cloud/ws/ wss://testnet-rpc2.cess.cloud/ws/` "wss://testnet-rpc3.cess.cloud/ws/"
        chainWsUrl: "ws://127.0.0.1:9944/"
        backupChainWsUrls: []
        # Bootstrap Nodes
        Boot: "_dnsaddr.boot-miner-testnet.cess.cloud"
        
      - name: "miner2"
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
        # each miner's mnemonic should be different
        mnemonic: "lllll kkkkk jjjjj iiiii hhhhh ggggg fffff eeeee ddddd ccccc bbbbb aaaaa"
        # miner work at this path
        diskPath: "/mnt/cess_storage2"
        # The rpc endpoint of the chain
        # `official chain: wss://testnet-rpc0.cess.cloud/ws/ wss://testnet-rpc1.cess.cloud/ws/ wss://testnet-rpc2.cess.cloud/ws/` "wss://testnet-rpc3.cess.cloud/ws/"
        chainWsUrl: "ws://127.0.0.1:9944/"
        backupChainWsUrls: []
        # Bootstrap Nodes
        Boot: "_dnsaddr.boot-miner-testnet.cess.cloud"
   ```

## 3. Generate configuration
   

The following command will generate `config.yaml` for each storage node and `docker-compose.yaml` based on the file located at: `/opt/cess/mineradm/config.yaml`.

   ```bash
   sudo mineradm config generate
   ```

- Generate each miner configuration at `$diskPath/miner/config.yaml`. For example, miner1's configuration generate at: `/mnt/cess_storage1/miner/config.yaml`
- Generate docker-compose.yaml at `/opt/cess/mineradm/build/docker-compose.yaml`

{% hint style="info" %}

If you want others server access to local rpc node, please add `--rpc-external` in `services.chain.command` in `/opt/cess/mineradm/build/docker-compose.yaml`

and add `--rpc-cors` `all` in `services.chain.command` to allow CORS: `Cross-Origin Resource Sharing`

- '--rpc-external'
- '--rpc-cors'
- 'all'

{% endhint %}

## 4. Installation

### Install all services
Install watchTower, rpc, and multi-miners services

  ```bash
  sudo mineradm install
  ```

### Skip install rpcnode
If an official RPC node or other known RPC node is configured in the configuration file, you can skip starting a local RPC node with `-skip-chain`.

  ```bash
  sudo mineradm install --skip-chain
  ```

## 5. Common Operations
**Stop all services**
```bash
  sudo mineradm stop
```

**Stop one or more specific service**

Such as execute `sudo mineradm stop miner1 miner2` to stop `miner1` and `miner2`

```bash
  sudo mineradm stop [miner name]
```

**Stop and remove all services**
```bash
  sudo mineradm down
```

**Stop and remove one or more specific service**

Such as execute `sudo mineradm down miner1` to remove `miner1`

```bash
  sudo mineradm down [miner name]
```

**Restart all services**
```bash
  sudo mineradm restart
```

**Restart one or more specific service**

Such as execute `sudo mineradm restart miner1` to restart `miner1`

```bash
  sudo mineradm restart [miner name]
```

**Get version information** 
```bash
  sudo mineradm version
```

**Check services status**
```bash
  sudo mineradm status
```

**Pull images**
```bash
  sudo mineradm pullimg
```

**Check disk usage** 
```bash
  sudo mineradm tools space-info
```

**View all storage node's status**

Please wait hours for data sync in storage node when you first run
```bash
  sudo mineradm miners stat
```

**Increase all storage node's stake**

Such as execute `sudo mineradm miners increase staking 4000` to increase all node's stake

```bash
  sudo mineradm miners increase staking $deposit_amount
```

**Increase a specific storage node's stake**

Such as `sudo mineradm miners increase staking miner1 4000`

```bash
  sudo mineradm miners increase staking $miner_name $deposit_amount
```

**Query all storage node's reward**
```bash
  sudo mineradm miners reward
```

**Claim all storage node's reward**
```bash
  sudo mineradm miners claim
```

**Claim a specific storage node's reward**

Such as `sudo mineradm miners claim miner1`

```bash
  sudo mineradm miners claim $miner_name
```

**Update an earnings account**

Such as `sudo mineradm miners update earnings cXxxx`

```bash
  sudo mineradm miners update earnings $earnings_account
```

{% hint style="warning" %}
The process of exiting the CESS network will last for hours, and forcing an exit in the middle of the process will make the storage node being punished.
{% endhint %}

**Make all storage nodes exit the network of cess**
```bash
  sudo mineradm miners exit
```

**Make a specific storage node exit the network of cess**

Such as `sudo mineradm miners exit miner1`

```bash
  sudo mineradm miners exit $miner_name
```

**Withdraw all storage node's stake**

After all storage nodes **has exited CESS Network** (see above), run
```bash
  sudo mineradm miners withdraw
```

**Withdraw a specific storage node's stake**

After this node **has exited CESS Network** (see above), run
```bash
  sudo mineradm miners withdraw $miner_name
```

**Remove the data in chain and miner**
```bash
  sudo mineradm purge
```

## 6. upgrade mineradm client

Upgrade the mineradm client by execute command as below:

```bash
cd /tmp
sudo wget https://github.com/CESSProject/mineradm/archive/latest.tar.gz -O /tmp/latest.tar.gz
sudo tar -xvf latest.tar.gz
cd mineradm-latest
sudo bash ./install.sh --no-rmi --retain-config --skip-dep --keep-running
```

After the program update is completed, please regenerate your configuration as below:

```bash
sudo cat /opt/cess/mineradm/.old_config.yaml > /opt/cess/mineradm/config.yaml
sudo mineradm config generate
```

Options help:
```text
    -n | --no-rmi              do not remove the corresponding image when uninstalling the old services
    -r | --retain-config       retain old config when upgrade mineradm
    -s | --skip-dep            skip install the dependencies
    -k | --keep-running        do not stop the services if cess services is running
```