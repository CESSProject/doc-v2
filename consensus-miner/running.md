# Server Requirement

The recommended requirement of a consensus server:

| Resource                      | Specification               |
| ----------------------------- | --------------------------- |
| Recommended OS                | Ubuntu\_x64 20.04 or higher |
| # of CPU Cores                | ≥ 4                         |
| Intel SGX Enabled             | required                    |
| Memory (SGX encrypted memory) | ≥ 64 GB                     |
| Bandwidth                     | ≥ 5 Mbps                    |
| Public Network IP             | required                    |
| Linux Kernel Version          | 5.11 or higher              |

{% hint style="info" %}
### SGX Enabled

The CPU must support [Intel Software Guard Extensions](https://www.intel.com/content/www/us/en/architecture-and-technology/software-guard-extensions.html) (Intel SGX) technology and Flexible Launch Control (FLC). The BIOS must support Intel SGX, and must enable the Intel SGX option. Please refer to the server manufacturer's BIOS guide to enable SGX functionality. Check out [CPU models that support SGX](https://ark.intel.com/content/www/us/en/ark/search/featurefilter.html?productType=873&2_SoftwareGuardExtensions=Yes). They can be either _Intel ME_, _Intel SPS_, or _both Intel SPS and Intel ME_.</br>

- CPU Recommended Models: Intel E, E3, Celeron (some models), Core series CPUs, with Intel Core i5-10500 being the optimal choice.
- Recommended Motherboard BIOS: Preferred options include mainstream manufacturers such as Supermicro.

### Fixed Public IP

The machine must use a fixed public network IP. The traffic exit must be in the same network segment as the IP. Execute the following command to confirm they are in the same network segment.

```bash
curl ifconfig.co
```
{% endhint %}

# Prepare CESS Account

## Decide in Which Capacity to Run the Consensus Miner

Starting from CESS v0.7.6, users can choose to run the consensus miner in the following capacities:

- **Full**：Full nodes possess all the functionalities, designed to be compatible with existing TEE Worker types. Registration requires `binding to consensus nodes`;

- **Verifier**：Verification nodes primarily handle idle and servicing random challenges. This type also needs to be `binding to consensus nodes` for registration;

- **Marker**：Authentication nodes are used to calculate tags for user-serviced files, process idle key generation, idle authentication, and idle replacement tasks. This type can be registered independently and serves a designated storage node cluster. **Running the consensus node in this capacity does not increase reputation points**；

Running the consensus miner in `Full` and `Verifier` capacities requires two separate accounts. If you already have your own Stash account or want to designate someone else's Stash account, you do not need to perform the **Bond Fund** operation below.

- **Stash Account**: Requires at least staking 3,000,000 TCESS, either from the node owner or delegated by other users, to run a consensus validator.

- **Controller Account**: Only one account is needed, and it is used for the gas fee of the registration transaction.

   Running the consensus miner in the `Marker` capacity requires only one account. There is no need to perform the **Bond Fund** operation below.

- **Controller Account**: Only one account is needed, and it is used for the gas fee of the registration transaction.

Please refer to the artcle [Creating CESS Accounts](../community/cess-account.md) for creating a CESS account, goto [CESS testnet faucet](https://cess.cloud/faucet.html) to get TCESS, or [contact us](../introduction/contact.md) to receive TCESS tokens for staking.

After the wallet account is created, navigate to [CESS Explorer](https://testnet.cess.cloud/).

## Bond Fund for Stash (Binding to the Consensus Node)

Choose **Network**, click **Staking** > **Accounts** > **Stash**

![Add a Stash](../assets/consensus-miner/running/consensus-pic1.png)

Select both **Stash Account**. _CESS
After version v0.7.6, the controller account has been removed from the binding fund operation._

Value bonded: At least 3,000,000 TCESS. In _payment destination_, select the second option **Stash Account as the reward receiving account (do not increase the amount at stake)**, which means that mining income will not automatically added to staking.

![Bond Fund](../assets/consensus-miner/running/consensus-pic2.png)

Click **Bond** -> **Sign and Submit** to link Stash Account and Controller Account

![Sign and Submit](../assets/consensus-miner/running/consensus-pic3.png)

Fund is bonded successfully!

![Bonded Fund Successfully](../assets/consensus-miner/running/consensus-pic4.png)

# Install CESS Client

The `cess-nodeadm` is a CESS node deployment and management program. It helps deploying and managing storage nodes, consensus nodes, and full nodes, simplifying the devOps for all CESS miners.

```bash
wget https://github.com/CESSProject/cess-nodeadm/archive/refs/tags/v0.5.4.tar.gz
tar -xvf v0.5.4.tar.gz
cd cess-nodeadm-0.5.4
sudo ./install.sh
```

{% hint style="info" %}
Check that you are using [the most updated version](https://github.com/CESSProject/cess-nodeadm/releases) of `cess-nodeadm`. Currently it is **v0.5.4**.
{% endhint %}

If a message `Install cess nodeadm success` shows up, the installation is successful.

If the installation fails, please check the [troubleshooting procedures](../storage-miner/troubleshooting.md).

# Config CESS Client

Run:

```bash
sudo cess config set
```

The following is an operational example of running the miner in the `Full` capacity:

*Tips: You can press Enter to skip when the default value of 'current' is suitable*

```bash
Enter cess node mode from 'authority/storage/watcher' (current: authority, press enter to skip): authority

## When you choose "authority," the Intel SGX driver on your machine will be started in software mode. You may see the prompt: "Software enable has been set. Please reboot your system to finish enabling Intel SGX." Please restart your machine after completing the configuration before proceeding to the next steps!

Begin install sgx_enable ...
Intel SGX is already enabled on this system
Enter cess node name (current: cess, press enter to skip): cess
Enter cess chain ws url (default: ws://cess-chain:9944):
Enter the public port for TEE worker (current: 19999, press enter to skip):
Start configuring the endpoint to access TEE worker from the Internet
  Try to get your external IP ...

## This step will automatically detect your machine's IP. If the automatic detection is incorrect, please manually enter the correct http://ip:port, where the port is the value you set in the previous step. Of course, you can also set the endpoint as a domain name.

Enter the TEE worker endpoint (current: http://xx.xxx.xx.xx:19999, press enter to skip)

## When current is set to null, it means it is empty. You can simply press Enter to skip if you want to become a Marker.

Enter cess validator stash account (current: null, press enter to skip): cXic3WhctsJ9cExmjE9vog49xaLuVbDLcFi2odeEnvV5Sbq4f
Enter what kind of tee worker would you want to be [Full/Verifier]: Full
Enter cess validator controller phrase: xxxxxxxxxxxxxx
Set configurations successfully
Start generate configurations and docker compose file
debug: Loading config file: config.yaml
info: Generating configurations done
info: Generating docker compose file done
e1f4b19325de7526801573bc31e04e3aa54cbac7af1971c2be83f8da0c16e85c
Configurations generated at: /opt/cess/nodeadm/build
try pull images, node mode: authority
download image: cesslab/cess-chain:testnet
testnet: Pulling from cesslab/cess-chain
96d54c3075c9: Already exists
224698f4f3fe: Already exists
fe59e467d907: Pull complete
4f4fb700ef54: Pull complete
Digest: sha256:39821a9755ecc0c8901809e8a29454ec618ac73592818d3829abdf73ded4e89e
Status: Downloaded newer image for cesslab/cess-chain:testnet
docker.io/cesslab/cess-chain:testnet
download image: cesslab/ceseal:testnet
testnet: Pulling from cesslab/ceseal
01085d60b3a6: Already exists
75b070fa4d64: Already exists
e0b98820ba1b: Pull complete
28557caa1da0: Pull complete
Digest: sha256:6d5c7b74a98208acc8a10ab833eef6c9a6977ed9b82e98aa08cdba732dd5ac05
Status: Downloaded newer image for cesslab/ceseal:testnet
docker.io/cesslab/ceseal:testnet
download image: cesslab/cifrost:testnet
testnet: Pulling from ccesslab/cifrost
96526aa774ef: Already exists
5c097a021ba1: Already exists
ff32bfaa56d6: Pull complete
Digest: sha256:7b0b1c04942d92cd69cac2a01f29ea7a889f9c5784c6af847152b8818fc946e5
Status: Downloaded newer image for cesslab/cifrost:testnet
docker.io/cesslab/cesslab/cifrost:testnet
pull images finished
```
Please fill in your TEE Worker server address while you configure the endpoint. The default is the address from the local server. If you do not know TEE Worker yet, please refer to the [node role introduction](../concepts/node-roles.md).

If the configuration process fails, please refer to the [troubleshooting guideline](../storage-miner/troubleshooting.md).

# Manage Validator Life Cycle

## Becoming a Validator

1. Start the consensus node<br/>

    ```bash
    cess start
    ```
2. Generate a session key<br/>

    ```bash
    cess tools rotate-keys
    ```

    ![rotate-keys Output Example](../assets/consensus-miner/running/rotate-keys.png)

    The field in the quotation marks after "result" is the Session Key, which will be used in subsequent operations. "localhost:9933" is the default port.<br/>

3. Setup a session key<br/>

    Navigate to [CESS Explorer](https://testnet.cess.cloud), choose **Network** > **Staking** > **Accounts** > **Session Key**<br/>

    ![Session Key 01](../assets/consensus-miner/running/session-key-01.png)

    Fill in the **Session Key** in the red box<br/>

    ![Session Key 02](../assets/consensus-miner/running/session-key-02.png)

    Click **Sign and Submit**<br/>

    ![Session Key 03](../assets/consensus-miner/running/session-key-03.png)

4. Becoming a validator<br/>

    Navigate to [CESS Explorer](https://testnet.cess.cloud), click **Network** > **Staking** > **Accounts** > **Validate**<br/>

    ![Validator 01](../assets/consensus-miner/running/validator-01.webp)

    ![Validator 02](../assets/consensus-miner/running/validator-02.png)

    Enter **100** in _reward commission percentage_, indicating that the reward will not be distributed to others.<br/>

    Select **No, block all nominations** in _allows new nominations_ dropdown, indicating that no nominations will be accepted.<br/>

    Again, click **Sign and Submit**.<br/>

    ![Validator 03](../assets/consensus-miner/running/validator-03.png)

    After completing the steps above, open the [CESS Explorer](https://testnet.cess.cloud/) and click **Network** > **Staking** > **Waiting**.<br/>

    ![Validator 04](../assets/consensus-miner/running/validator-04.webp)

    You should see that the node has already appeared on the candidate node list.

## Redeeming Rewards

Navigate to CESS Explorer: **Network** > **Staking** > **Payouts** > **Payout**.

![Redemption: First Step](../assets/consensus-miner/running/redemption-01.png)

In Payouts, click **Payout** to initiate a payment. Any account can initiate a payment.

![Redemption: Second Step](../assets/consensus-miner/running/redemption-02.png)

{% hint style="info" %}
Please claim the reward within 84 era (each era of the test network is 24 hours), which is 84 days. Those who hasn't claimed the reward in this period will not be able to claim it.
{% endhint %}

## Exiting Consensus Validation

1. Stop the Consensus<br/>

    In [CESS Explorer](https://testnet.cess.cloud), navigate to: **Network > Staking > Account Actions > Stop**.<br/>

    ![Exiting-01](../assets/consensus-miner/running/exiting-01.png)

2. Clear Session Keys<br/>

    In [CESS Explorer](https://testnet.cess.cloud), navigate to: **Developer -> Submission**<br/>

    ![Exiting-02](../assets/consensus-miner/running/exiting-02.png)

    Enter controller account in _using the selected account controller_. Then in _submit the following extrinsic_, enter **session** and choose **purgeKeys()** in the box next to it.<br/>

    ![Exiting-03](../assets/consensus-miner/running/exiting-03.png)

    Click **Submit Transaction** button to clear session keys<br/>

    ![Exiting-04](../assets/consensus-miner/running/exiting-04.png)

## Redeeming the Stake

1. Unbond fund<br/>

    After 28 eras (each era of the test network is 24 hours), goto [CESS Explorer](https://testnet.cess.cloud/), navigate to: **Network > Staking > Account Actions > Unbond Funds**.<br/>

    ![Staking 01](../assets/consensus-miner/running/staking-01.png)

2. Stop the CESS client<br/>

    ```bash
    cess stop
    ```

# Common Operations

## Start Consensus Node

```bash
cess start
```

## Query Miner Status

```bash
$ cess status

-----------------------------------------
 NAMES           STATUS
cifrost         Up 2 minutes
ceseal          Up 2 minutes
chain           Up 2 minutes
watchtower      Up 2 minutes
-----------------------------------------
```

## Examine Config Information

```bash
cess config show
```

## Stop and Remove All Services

```bash
cess down
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
wget https://github.com/CESSProject/cess-nodeadm/archive/refs/tags/<new-version>.tar.gz
tar -xvf <new-version>.tar.gz
cd cess-nodeadm-<new-version>
./install.sh --skip-dep
```

Currently [the most updated version](https://github.com/CESSProject/cess-nodeadm/tags) is **v0.5.4**.

## Pull Images

```bash
cess pullimg
```

# Questions & Answers

1. I don't want to expose my IP address on the chain. What should I do?</br>

   During the cess config set process, you can set your endpoint with a domain name. For example, if your registered domain is tee-xxx.cess.cloud, you can enter http://tee-xxx.cess.cloud when setting the endpoint. The script will then ask you if you want to enable one-click domain proxy. You can enter y to enable it, as shown below:

   ```bash
   .....
   Enter the kaleido endpoint (current: http://tee-xxx.cess.cloud, press enter to skip): http://tee-xxx.cess.cloud
   Do you need to configure a domain name proxy with one click? (y/n): y
   .....
   ```

   Alternatively, you can manually configure an nginx proxy. Please avoid using the intermediate proxy provided by the domain service provider.

2. How do I know if the program is working properly?</br>

   You can select Chain State in the block explorer. Through this method, you can check whether the registration was successful.

   ![check-register](../assets/consensus-miner/qa/check-register.png)

3. I don't want the program to update automatically. What should I do?</br>

   After the program has started successfully, a watchtower service will manage local services on behalf of the user. When the CESS official updates a component, the watchtower will pull the latest program for automatic upgrading. If you don't want to use the automatic upgrade feature, you can disable it with the following command before the cess config set.

   ```bash
   ## Disable the update of the ceseal service.
   cess tools no_watchs ceseal

   ## Disable the update of the cifrost service.
   cess tools no_watchs cifrost
   ```

   Every automatic upgrade from you means a bug fix for the consensus miner program by the official, and we **strongly discourage** you from turning off the automatic upgrade feature, as this may render your service **unusable**.
