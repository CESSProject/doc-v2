As an indispensable part of the blockchain, transactions provide the mechanism to modify the information state stored on the chain. Each transaction history will be recorded in the block. The transactions have the following three types:

- Signed transaction
- Unsigned transaction
- Inherent transaction

# Signed Transaction

In CESS network, signed transaction is the most common transaction type. When sending a signed transaction request, the signature of the requesting account must be attached. Usually, this is achieved by signing the transaction content using the private key of the requesting account. In addition, the requesting account also needs to pay transaction fees. The processing method of transaction fees and other elements depends on the program logic.

For a signed transaction, only the sender needs to pay the transaction fee. For example, if you need to transfer a certain number of tokens from your account to others, you can invoke the [`transfer`](https://paritytech.github.io/substrate/master/pallet_balances/pallet/struct.Pallet.html#method.transfer) extrinsic in [**Balances Pallet**](https://paritytech.github.io/substrate/master/pallet_balances). Since your account calls this transaction, you should use your account private key to sign the transaction. At the same time, in the process of sending the request, you can choose to attach a tip to raise the processing priority of this transaction in the network.

# Unsigned Transaction

The vast majority of transactions are signed transactions. Only a few unsigned transactions are not initiated by the user, but are requested by the network through the off-chain worker.

No fees will be charged for unsigned transactions, and no information of the requester is required. This means that there are no economic restrictions on unsigned transactions, thus spam requests or replay attacks may occur.

Therefore, the checking of unsigned transactions before execution must be very restrictive. In addition, since unsigned transactions are user-defined verification processes, they usually consume more resources than signed transactions.

In CESS network, only the validator of current rotation can be the initiator of an unsigned transaction. The request content needs to be attached with the signature digest of the specific account in order to pass the user-defined verification in CESS network.

There is also a module using unsigned transactions in Substrate framework, namely [`pallet_im_online::pallet::Call::heartbeat`](https://paritytech.github.io/substrate/master/pallet_im_online/pallet/struct.Pallet.html#method.heartbeat) extrinsic. This transaction enables the validator node to send a message to the network to confirm that the node is online. For this transaction, Substrate has a strict verification process, and only accounts registered as validator nodes are allowed to send.

# Inherent Transaction

Inherent transaction is a special type of unsigned transaction. Through this type of transaction, the block producing node can directly insert information to the block. Generally, this type of transaction will not be gossiped to other nodes or added to the transaction queue. We will consider it valid without specific verification.

In CESS network, inherent transaction is not used. But in Substrate framework, the [`pallet_timestamp::pallet::Call::set`](https://paritytech.github.io/substrate/master/pallet_timestamp/pallet/struct.Pallet.html#method.set) transaction uses the Intrinsic type to insert the current timestamp to the produced block. Although the other nodes cannot confirm whether the timestamp contained in the block information is correct, they can determine whether the timestamp is within an acceptable range. If this is not the case, they reject the block.

# Transaction Fee and Weight

When executing transactions and uploading data on the chain, it will change the state of the chain and consume blockchain resources. As mentioned earlier, because the resources in a blockchain are scarce, how to use and manage resources become important. Besides the fixed resource of storage capacity, there may also be malicious users sending a large number of messages to cause network overload, thus preventing the network from producing blocks.

To prevent resources abuse on blockchain, **transaction weight** and **transaction fees** are devised.

The relationship among transaction weight and transaction fee is as follows: The time taken for transaction execution is expressed as weight. Weight affects the amount of transaction fee. The greater the weight, the more expensive the transaction fee is.

Transaction fees provide economic incentives to limit execution time and the number of calls required to calculate and execute operations. Transaction fees are also used to make blockchain economically sustainable (some of them will be returned to the treasury pool and distributed as a new round of token), because they are usually applied to transactions initiated by users and deducted before executing transaction requests.

{% hint style="success" %}
### Final Transaction Fee Formula

**Inclusive fee** = basic fee + byte length fee + (target fee adjustment \* weight)

**Final Transaction fee** = inclusive fee + tip
{% endhint %}

**Basic fee**: the fee is the minimum amount that the user pays for the transaction.

**Byte length fee**: the fee proportional to the length of the transaction code. The fee for each byte is multiplied by the length of the transaction code.

**Target fee adjustment**: a multiplier that can be adjusted according to network congestion.

**Weight**: the transaction weight.

**Tip**: discretionary amount from senders to speed up their transaction processing.
