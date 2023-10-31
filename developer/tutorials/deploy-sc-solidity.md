# Overall

In this tutorial, we will learn how to deploy a [**Solidity** Smart Contract](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html) on the CESS blockchain. Solidity smart contracts are widely deployed on EVM-compatible chains, notably Ethereum. CESS blockchain is also EVM-compatible and allows Solidity developers to deploy their contracts on CESS with no or minimal changes.

# Preparation

You will need the following to deploy a Solidity smart contract to CESS.

- **MetaMask**: Required to get an Ethereum address and to connect to the CESS chain
- **CESS Acount**: Refer to [this article](../../community/cess-account.md) on how to create a CESS account and [this article](../guides/testnet-faucet.md) on getting testnet tokens from our faucet.
- **Remix IDE**: Access to [Remix IDE](https://remix.ethereum.org/) to develop, compile, and deploy smart contracts to the chain
- **Access to CESS Node**: Make sure the node allows access to MetaMask

The following steps will guide you to deploy a Solidity contract on the CESS testnet.

{% hint style="info" %}
This tutorial involves working with the EVM in CESS test-chain. You will understand better if you understand the reasoning and mechanism behind the [Subtrate and EVM address conversion](../guides/substrate-evm.md).
{% endhint %}

# Add CESS Network to MetaMask

Open the MetaMask setting tab, click on the **Networks** tab, click on **Add a network** and then **Add a network manually**.

![Metamask: Adding a Network](../../assets/developer/tutorials/deploy-sc-solidity/01.png)

On **Add a network manually** page, enter the following details:

- Network Name: **CESS Testnet**
- New RPC URL, one of the following:
   - <https://testnet-rpc0.cess.cloud/ws/>
   - <https://testnet-rpc1.cess.cloud/ws/>
   - <https://testnet-rpc2.cess.cloud/ws/>
- Chain ID: **11330**
- Currency Symbol: **TCESS**

![Metamask: Adding CESS Testnet](../../assets/developer/tutorials/deploy-sc-solidity/02.png)

# Convert the Substrate Address to an EVM Address

Copy the account address from MetaMask.

![Metamask: My Account](../../assets/developer/tutorials/deploy-sc-solidity/03.png)

Open the page to [Substrate Address Converter](https://hoonsubin.github.io/evm-substrate-address-converter).

![Substrate Address Converter](../../assets/developer/tutorials/deploy-sc-solidity/04.png)

Input the following:

- Current Address Scheme: **H160**
- Change Address Prefix: **11330*
- Intput address: *your metamask account*

Get the Substrate address output.

# Fund the Account

Using [CESS Explorer](https://testnet.cess.cloud/) **Accounts -> Transfer** to transfer some balance to the Substrate address output above. For testnet goto [testnet faucet](https://cess.cloud/faucet.html) to get fund drip into this Substrate address.

![CESS Explorer: Transfer Amount](../../assets/developer/tutorials/deploy-sc-solidity/05.png)

# Validate the Fund

To validate the funds are in the Ethereum account, open MetaMask and check that account has the funds transferred

![Metamask: Check My Account](../../assets/developer/tutorials/deploy-sc-solidity/06.png)

# Deploy a Contract Using Remix IDE

Open [Remix IDE](https://remix.ethereum.org/) and go to **File explorer**.

In File explorer, open the smart contract you wish to compile and then deploy.

![Remix: Deploy a Contract](../../assets/developer/tutorials/deploy-sc-solidity/07.png)

Once the file is selected, go to tab **Solidity Compiler**, you should see the selected file, press the **Compile** button to compile the contract. Once compiled, you’ll see the "green tick" mark and compiled (\*.sol) file.

![Remix: Contract Compiled](../../assets/developer/tutorials/deploy-sc-solidity/08.png)

Go to **Deploy and run Transactions**, once the compilation is successful, you should see the compiled \*.sol file selected, ready to be deployed. In the **Environments** drop-down, select **Injected Provider - MetaMask** and click **deploy**.

![Remix: Deploy Contract to CESS](../../assets/developer/tutorials/deploy-sc-solidity/09.png)

{% hint style="info" %}
When you click **Deploy**, you will need to confirm in MetaMask to allow Remix to access the account and submit the transaction.
{% endhint %}

Click **Confirm** to submit the transaction to deploy the smart contract.

![Metamask: Confirm Deploy Transaction](../../assets/developer/tutorials/deploy-sc-solidity/10.png)

After the transaction is deployed and processed on-chain, you’ll see the following message.

![Remix: Deployment Succeeded](../../assets/developer/tutorials/deploy-sc-solidity/11.png)

In the **Deployed Contracts** section in the Remix, you can call the function of the smart contract.

![Remix: Interact with the Contract](../../assets/developer/tutorials/deploy-sc-solidity/12.png)

# Transfer Tokens to the CESS Account

Convert the Substrate address to Ethereum account address using the link [Substrate Address Converter](https://hoonsubin.github.io/evm-substrate-address-converter).

![Substrate-EVM Address Converter](../../assets/developer/tutorials/deploy-sc-solidity/13.png)

Copy the Ethereum equivalent address and use MetaMask to transfer fund.

<table>
  <tr>
    <td>
      <img src="../../assets/developer/tutorials/deploy-sc-solidity/14.png" alt="deploy-sc-solidity-14"/>
      <br/>Transfer Fund in Metamask 1
    </td>
    <td>
      <img src="../../assets/developer/tutorials/deploy-sc-solidity/15.png" alt="deploy-sc-solidity-15"/>
      <br/>Transfer Fund in Metamask 2
    </td>
  </tr>
</table>

Confirm the balance in the [CESS Explorer: Developer RPC calls](https://testnet.cess.cloud/#/rpc). Use the Ethereum address in previous step.

![CESS Explorer: Get Balance](../../assets/developer/tutorials/deploy-sc-solidity/16.png)

# Withdraw Balances to the CESS Account

To withdraw the balance from the Ethereum account to CESS account, follow the route **Developer => Extrinsics => evm => withdraw**.

![CESS Explorer: Sending evm:withdraw Transaction](../../assets/developer/tutorials/deploy-sc-solidity/17.png)

Validate the balances in **Accounts** tab of CESS Explorer.

![CESS Explorer: Account Updated](../../assets/developer/tutorials/deploy-sc-solidity/18.png)
