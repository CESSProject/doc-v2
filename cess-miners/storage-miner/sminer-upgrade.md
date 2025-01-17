# Introduction

**For better data transmission, we need to upgrade our storage miner to latest version which is incompatible with the old version.**

Add configuration: `apiendpoint` and `timeout`
- apiendpoint: An external `ip:port` or `domain` which can be accessed by public network, default value: `hostPublicIP:port`
- Timeout: Default `12` seconds for transaction with chain.

Delete configuration: `Boot`

## Old Configuration File Schema

```yaml
Name: miner1
Port: 15001
EarningsAcc: cXxxx
StakingAcc: cXxxx
Mnemonic: expand left depict favorite busy marriage good curtain celery misery fly obscure
Rpc:
  - 'ws://127.0.0.1:9944'
  - 'wss://testnet-rpc.cess.network'
UseSpace: 1000
Workspace: /opt/miner-disk
UseCpu: 2
TeeList:
  - '127.0.0.1:8080'
  - '127.0.0.1:8081'
Boot:
  - _dnsaddr.boot-miner-testnet.cess.network
```

## New Configuration File Schema
```yaml
app:
  workspace: /opt/miner-disk
  port: 15001
  maxusespace: 1000
  cores: 2
  apiendpoint: '1.1.1.1:15001'
chain:
  mnemonic: expand left depict favorite busy marriage good curtain celery misery fly obscure
  stakingacc: null
  earningsacc: cXxxx
  timeout: 12
  rpcs:
    - 'ws://127.0.0.1:9944'
    - 'wss://testnet-rpc.cess.network'
```


# How to Upgrade
## For nodeadm user
```bash
# step 1: update nodeadm client
wget https://github.com/CESSProject/cess-nodeadm/archive/vx.x.x.tar.gz
tar -xvf vx.x.x.tar.gz
cd cess-nodeadm-x.x.x
sudo ./install.sh --skip-dep

# step 2: update related images
$ sudo cess pullimg

# step 3: update config file
$ sudo cess config set
# Start configuring the endpoint to access Storage-Miner from the internet
# Do you need to automatically detect extranet address as endpoint? (y/n)  need_detect
# ...

# step 4: restart service
$ sudo cess restart
```

## For mineradm user

```bash
# step 1: update mineradm client
sudo wget https://github.com/CESSProject/cess-multiminer-admin/archive/latest.tar.gz -O /tmp/latest.tar.gz
sudo tar -xvf latest.tar.gz
cd cess-multiminer-admin-latest
sudo bash ./install.sh --no-rmi --retain-config --skip-dep --keep-running

# step 2: update related images
$ sudo mineradm pullimg

# step 3: update config file
# Attention: Storage miner will not use public tee nodes on chain if set custom tee nodes in config.yaml
method 1: edit /opt/cess/mineradm/config.yaml based on the old config file: /opt/cess/mineradm/.old_config.yaml
method 2: use default value of apiendpoint/Timeout: cat /opt/cess/mineradm/.old_config.yaml > /opt/cess/mineradm/config.yaml

# step 4: generate new config file
$ sudo mineradm config generate

# step 5: restart service
$ sudo mineradm down
$ sudo mineradm install
```

