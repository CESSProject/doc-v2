**Random Rotational Consensus** is an important component of the CESS protocol. Unlike the Polkadot consensus mechanism, random rotation consensus changes the process of verifying node elections. The following is the overall process flow:

1. A node stakes 3 million tokens to register as a consensus node.

2. At the start of each era, validator nodes are chosen based on credit ratings. The top 11 nodes with the highest credits among all consensus nodes serve as the validation nodes for that era.

3. The final credit rating is calculated based on reputation credits, staked score (calculated from both self and delegated), and random credits.

{% hint style="success" %}
$$
Final Credits = (reputation Credits * 50\%) + (staked Score * 30\%) + (random Credits * 20\%)
$$
{% endhint %}

5. The block producer selection mechanism is the same as [BABE](https://wiki.polkadot.network/docs/learn-consensus#block-production-babe), where each block producer is randomly selected from 11 validation nodes through [VRF](https://doc.cess.network/ref/in-depth-feat/vrf).

6. The method of confirming blocks is the same as [GRANDPA](https://wiki.polkadot.network/docs/learn-consensus#finality-gadget-grandpa).

7. At the 5th epoch of each era, the validator node of the next era will be selected.

# Reputation Model

Each consensus node entering the CESS network needs to maintain the network state and also undertake the task of data storing and scheduling. In order to encourage consensus nodes to do more, we have designed a reputation model. In our model, each consensus node has a reputation value, which is directly determined by the workload of the scheduler. Specifically, it includes the following items:

1. The total number of bytes processed for service files.

2. Number of timeout penalties for random file validation challenges.

The reputation value is calculated as follows:

{% hint style="success" %}
$$
Scheduler Reputation Value = 1000 * processing Bytes Ratio - (10 * penalty Times)
$$
{% endhint %}
