Random rotational consensus is an important component of the CESS protocol. Unlike the Polkadot consensus mechanism, random rotation consensus changes the process of verifying node elections. The following is the overall process flow:

1. A node stakes 1 million tokens to register as a consensus node.

2. At the beginning of each era validation nodes will rotate, and the rotation rule is credit ranking: Selecting the top 11 nodes with the highest credits from all consensus nodes as the validation nodes for this era.

3. The final credit is determined by the sum of reputation credits and the random credits:

    {% hint style="success" %}
    **Final Credits** = (reputation credits * 80%) + (random credits * 20%).
    {% endhint %}

4. The block producer selection mechanism is the same as [BABE](https://wiki.polkadot.network/docs/learn-consensus#block-production-babe), where each block producer is randomly selected from 11 validation nodes through VRF.

5. The method of confirming blocks is the same as [GRANDPA](https://wiki.polkadot.network/docs/learn-consensus#finality-gadget-grandpa).

6. At the 5th epoch of each era, the validator node of the next era will be selected.

# Reputation Model

Each consensus node entering the CESS network needs to maintain the network state and also undertake the task of data storing and scheduling. In order to encourage consensus nodes to do more, we have designed a reputation model. In our model, each consensus node has a reputation value, which is directly determined by the workload of the scheduler. Specifically, it includes the following items:

1. The total number of bytes processed for service files.

2. Number of timeout penalties for random file validation challenges.

The reputation value is calculated as follows:

{% hint style="success" %}
**Scheduler Reputation Value** = 1000 * processing bytes ratio - (10 * penalty times)
{% endhint %}
