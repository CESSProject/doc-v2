Reputation rotational consensus is an important component of CESS protocol. Different from the Polkadot consensus mechanism, reputation rotation consensus changes the process of verifying node elections. The following is the overall process flow:

1. A node stakes 1 million tokens to register as a consensus node.

2. Each era validation node will rotate, and the rotation rule is credit ranking. Select the top 11 nodes with the highest credits (4 nodes in the CESS test network) from all consensus nodes as the validation nodes for this era.

3. The final credit is determined by the reputation credits and the random credits:

    {% hint style="success" %}
    **Final Credits** = (reputation credits * 80%) + (random credits * 20%).
    {% endhint %}

4. Please refer to the next chapter for reputation credits calculation, where random credits are generated through VRF.

5. Block producer selection mechanism is the same as BABE, where each block producer is randomly selected from 11 validation nodes through VRF.

6. The method of confirming blocks is the same as GRANDPA.

7. At the 5th epoch of each era, the validator node of the next era will be selected.

# Reputation Model

Each consensus node entering the CESS network needs to maintain the network state and also undertake the task of data storing and scheduling. In order to encourage consensus nodes to do more, we have designed a reputation model. In our model, each consensus node has a reputation value, which is directly determined by the workload of the scheduler. Specifically, it includes the following items:

1. The total number of bytes processed for service files.

2. Number of timeout penalties for random file validation challenges.

The reputation value is calculated as follows:

{% hint style="success" %}
**Scheduler Reputation Value** = 1000 * processing bytes ratio - (10 * penalty times)
{% endhint %}
