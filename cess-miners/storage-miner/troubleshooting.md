# Issues During Installation

<details>

<summary>Unable to download docker image</summary>

During the installation process, docker is used to download cess image. If the following error occurs when installing the `cess-nodeadm`:

![Docker Daemon Issue](../assets/storage-miner/troubleshooting/docker-daemon-issue.png)

Make sure commands are in the root privilege or prefixed with `sudo` command. Start docker on your system:

```bash
systemctl start docker
```

Reinstall the `cess-nodeadm`:

```bash
./install.sh
```

⚠️ Note that most CESS program commands must have root privileges.

</details>

<details>

<summary>Failed to locate docker package</summary>

If the following error occurs when installing the `cess-nodeadm`:

![Docker Package Issue](../assets/storage-miner/troubleshooting/docker-package-issue.webp)

Try to delete Docker with following commands:

```bash
sudo systemctl stop docker
docker stop $(docker ps -aq)
docker rm -v $(docker ps -aq)
docker rmi $(docker images -aq)
docker volume rm $(docker volume ls -q)
brew uninstall docker
```

Reinstall Docker:

```bash
sudo apt-get install docker-ce
sudo systemctl enable docker
sudo systemctl start docker
```

</details>

# Issues After Installation

<details>

<summary>Increase Stake Manually</summary>

If signatureAcc different from stakingAcc is provided as below:
![CESS Account Issue](../assets/storage-miner/troubleshooting/different-acc.png)

You can not increase stake by command with client:
```bash
sudo cess storage node increase staking $deposit_amount
# or
sudo mineradm miners increase staking $miner_name $deposit_amount

# Execute command as above might get message like: `!! 2024-03-28 13:22:18 0xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
```

Try to access to [block browser](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ftestnet-rpc.cess.network%2Fws%2F#/accounts) and send TCESS manually

**Step 1**: Select an account which have sufficient TCESS, then click `send`
![CESS Account Issue](../assets/storage-miner/troubleshooting/send-in-browser.png)

**Step 2**: Enter the staking account and amount, then click `Make Transfer`
![CESS Account Issue](../assets/storage-miner/troubleshooting/make-transfer-in-browser.png)

**Step 3**: Finally, enter the password for the account you have selected that has sufficient TCESS.

</details>



# Issues During Configuration

<details>

<summary>Failed to download CESS image</summary>

If the following error occurs when setting up the config:

![CESS Image Download Issue](../assets/storage-miner/troubleshooting/cess-image-download-issue.png)

Ensure the commands are run in the root privilege or prefixed with `sudo` command.

Try `cess config set` command.

</details>

<details>

<summary>Invalid config file (config.yaml)</summary>

![Invalid Config Issue](../assets/storage-miner/troubleshooting/invalid-config-issue.webp)

Delete file `/usr/bin/yq`:

```bash
sudo rm /usr/bin/yq
```

Reinstall `cess-nodeadm` again:

```bash
./install.sh
```

</details>

<details>

<summary>Set Docker Daemon Access with TLS</summary>

`mineradm` will enable docker daemon access at port: `2375` automatically when install `mineradm`, but if you want to watchdog access to a host in public network, you need to set that host's docker daemon start with TLS.

Because watchdog need to request each storage node's config file from others hosts by docker api, and this config file contain storage node's mnemonic, so it must encrypt when transferring in public network.

It is a shell demo to generate files by openssl. change the `<IP where watchdog run>` to your watchdog server ip. You can get more detail information from [Docker Daemon Access with TLS](https://docs.docker.com/engine/security/protect-access/).

**Please keep your file safe and make sure no one can get your key file.**

```bash
PASSPHRASE=
openssl genrsa -aes256 -passout pass:$PASSPHRASE -out ca-key.pem 4096
openssl req -new -x509 -passin pass:$PASSPHRASE -days 36500 -key ca-key.pem -sha256 -subj "/C=US" -out ca.pem
openssl genrsa -aes256 -passout pass:$PASSPHRASE -out server-key.pem 4096
openssl req -subj "/C=US" -passin pass:$PASSPHRASE -passout pass:$PASSPHRASE -sha256 -new -key server-key.pem -out server.csr
echo subjectAltName = DNS:IP:<IP where watchdog run> >> extfile.cnf
echo extendedKeyUsage = serverAuth >> extfile.cnf
openssl x509 -req -days 36500 -passin pass:$PASSPHRASE -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
openssl genrsa -out key.pem 4096
openssl req -subj '/CN=client' -new -key key.pem -out client.csr
echo extendedKeyUsage = clientAuth > extfile-client.cnf
openssl x509 -req -days 36500 -passin pass:$PASSPHRASE -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile-client.cnf
openssl rsa -passin pass:$PASSPHRASE -in server-key.pem -out server-key-decrypted.pem
rm -v client.csr server.csr extfile.cnf extfile-client.cnf
chmod -v 0444 ca.pem server-cert.pem cert.pem
```

After generate files by openssl, start listen docker daemon with TLS at port: 2376

```bash
# Testing: docker can run with tls successfully
systemctl stop docker
dockerd --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key-decrypted.pem -H=0.0.0.0:2376 -H unix:///var/run/docker.sock &
```

Recommend to use `systemd` to start docker daemon with TLS.
```bash

# 1: edit file: /lib/systemd/system/docker.service

# 2: modify row: `ExecStart=...` to
ExecStart=/usr/bin/dockerd --tlsverify --tlscacert=/etc/docker/ca.pem --tlscert=/etc/docker/server-cert.pem --tlskey=/etc/docker/server-key-decrypted.pem -H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock

systemctl daemon-reload && systemctl restart docker

```


Finally, copy files(ca.pem/key.pem/cert.pem) to the server where watchdog run, then config the files path in `/opt/cess/mineradm/config.yaml` and run `mineradm config generate`

⚠️ Expose Docker API Port at `0.0.0.0:2375` without TLS is unsafe, it may get network attack like `kdevtmpfsi`

If you have already get attack, please execute command as down below

```bash

docker stop $(docker ps -a | grep ubuntu | awk '{print $1}')
docker rm $(docker ps -a | grep ubuntu | awk '{print $1}')
docker rmi $(docker images | grep ubuntu | awk '{print $3}')


sudo sed -i 's/^ExecStart=.*/ExecStart=\/usr\/bin\/dockerd -H fd:\/\/ -H unix:\/\/\/var\/run\/docker.sock -H tcp:\/\/127.0.0.1:2375/' /lib/systemd/system/docker.service
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo mineradm install
```
</details>
