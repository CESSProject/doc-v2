# Server Requirement

The recommended requirement of a storage server:

| Resource  | Specification |
| ------------- | ------------- |
| Recommended OS | Linux 64-bit Intel / AMD |
| # of CPU Cores | ≥ 4 |
| Memory | ≥ 8 GB |
| Bandwidth | ≥ 5 Mbps |
| Public Network IP | required |
| Linux Kernel Version | 5.11 or higher |

# Server Preparation

## Install Docker

Please refer to the [official documentation](https://docs.docker.com/engine/install/) for Docker installation.

## Firewall Configuration

{% hint style="info" %}
The following commands are executed with root privileges. If error messages of `permission denied` appear, switch to root privilege or add `sudo` at the beginning of these commands.
{% endhint %}

By default, the node client program, **cess-bucket**, uses port 4001 to listen for incoming connections, if your OS firewalled the port by default, you may need to enable the access to the port.

```bash
ufw allow 4001
```

## Disk Mounting

Check the hard disk status using the `df -h` command:

```bash
$ df -h
```

If the disk is not mounted, the hard drive for storage mining cannot be used. Use the commands below to view unmounted hard disks:

```bash
$ fdisk -l

Disk /dev/vdb: 200 GiB, 214748364800 bytes, 419430400 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x331195d1
```

From the above, we can see that the unmounted disk is `/dev/vdb`. We will be using `/dev/vdb` to demonstrate the mounting operation.

Allocate the `/dev/vdb` disk:

```bash
fdisk /dev/vdb
Enter and press Enter:
n
p
1
2048
the value after default
w
```

Format the newly divided disk into ext4 format:

```bash
mkfs.ext4 /dev/vdb
```

Enter "y" to continue if the system asks to proceed:

```bash
Proceed anyway? (y,N) y
```

Create `/cess` directory to mount the disk. Using `/cess` as an example:

```bash
mkdir /cess
echo "/dev/vdb /cess ext4 defaults 0 0" >> /etc/fstab
```

Replace `/dev/vdb` with your own disk name. /cess has to remain the same as created in the previous step. If you are not under root privileges, try:

```bash
echo "/dev/vdb /cess ext4 defaults 0 0" | sudo tee -a /etc/fstab
```

Mount `/cess`:

```bash
mount -a
```

Check the disk mounting status:

```bash
df -h
```

If `/cess` appears, the disk has been successfully mounted.

## Prepare CESS Accounts

Miners need to create two wallet accounts.

- **Earning Account**: Used to receive mining rewards.
- **Staking account**: Used to pay for staking fees.
- **Signature Account**: Used to sign blockchain transactions. If no staking account is specified, this account will also be used to pay staking fees.

Please refer to [Creating CESS Accounts](../community/cess-account.md) for creating a CESS account, goto [CESS faucet](https://cess.cloud/faucet.html) to get our testnet tokens, TCESS, or [contact us](../introduction/contact.md) to get assistance.

# Install CESS Client

1. Check for the latest version at：https://github.com/CESSProject/cess-nodeadm/tags
```
⚠️ According to the latest version number, replace the following x.x.x
```
2. Download and install
```bash
wget https://github.com/CESSProject/cess-nodeadm/archive/vx.x.x.tar.gz
tar -xvf vx.x.x.tar.gz
cd cess-nodeadm-x.x.x/
./install.sh
```

If a message `Install cess nodeadm success` shows up, the installation is successful.

If the installation fails, please check the [troubleshoot procedures](./troubleshooting.md).

# Stop and remove old services
stop old services：
```
cess stop
```
or
```
cess down
```
remove old services：
```
cess purge
```

# Configure CESS Client
## Set up a running network
switch to development network：
```
cess profile devtnet
```
switch to test network：
```
cess profile testnet
```

## Set up configuration

```bash
$ cess config set

Enter cess node mode from 'authority/storage/watcher' (current: watcher, press enter to skip): storage
Enter cess storage listener port (current: 15001, press enter to skip): 
Enter cess storage earnings account (current: cXiqKzVVamJ2d5cMKomh1ED4prAnKevr2v3nZgNH87HRuY4Xy, press enter to skip): 
Enter cess storage staking signature phrase (current: situate double coral cycle ritual country rebuild ridge slush smoke verb acquire, press enter to skip): 
Enter cess storage disk path (current: /mnt/storage-disk, press enter to skip): 
Enter cess storage space, by GB unit (current: 300, press enter to skip): 
Enter the number of CPU cores used for mining; Your CPU cores are 4
  (current: 3, 0 means all cores are used; press enter to skip): 
Set configurations successfully
```

Start CESS bucket

```bash
$ cess start

[+] Running 3/0
 ✔ Container chain       Running                                                0.0s
 ✔ Container bucket      Running                                                0.0s
 ✔ Container watchtower  Running                                                0.0s
```

# Common Operations

## Check CESS Chain Sync Status

```bash
docker logs chain
```

As shown below, if we see that the height of the block corresponding to "best" is about the latest height in [CESS Explorer](https://testnet.cess.cloud/), it means the local chain node synchronization is completed.

![CESS Blockchain Synchronization Completed](../assets/storage-miner/running/sync-status.png)

Only when the chain synchronization is completed can you operate other functions such as increase the staking, view the status of the node, etc.

## View the Storage Node Log

```bash
docker logs bucket
```

As shown below, seeing `/kldr-testnet` indicates that the network environment is a test network, and seeing `Connected to the bootstrap node...` indicates that there is a connection to the bootstrap node.

![Storage Node Log](../assets/storage-miner/running/view-node-log.webp)

## View Bucket Status

```bash
cess bucket stat
```

An example of the returned results is shown below：

![CESS Bucket Stat](../assets/storage-miner/running/bucket-stat.png)

Refer to the [Glossary](../glossary.md#storage-miner) on the names above.

## Increase Miner Staking

```bash
cess bucket increase <deposit amount>
```

## Withdraw Miner Staking

After your node **has exited CESS Network** (see below), run

```bash
cess bucket withdraw
```

## Query Reward Information

```bash
cess bucket reward
```

## Claim Reward

```bash
cess bucket claim
```

## Update All Service Images

```bash
cess pullimg
```

## Stop and Remove All Services

```bash
cess down
```

## Update Earnings Account

```bash
cess bucket update earnings [earnings account]
```

## Exit CESS Network

```bash
cess bucket exit
```

# Upgrade CESS Client

## Stop and Remove All Services

```bash
cess stop
cess down
```

## Remove All Chain Data

{% hint style="warning" %}
Do not perform this operation unless the CESS network has been redeployed and it is confirmed that the data can be cleared.
{% endhint %}

```bash
cess purge
```

## Update `cess-nodeadm`

```bash
wget https://github.com/CESSProject/cess-nodeadm/archive/vx.x.x.tar.gz
tar -xvf vx.x.x.tar.gz
cd cess-nodeadm-x.x.x
./install.sh --skip-dep
```

## Pull images

```bash
cess pullimg
```
