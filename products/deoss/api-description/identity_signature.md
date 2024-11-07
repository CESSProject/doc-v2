## Identity signature
calling some APIs requires authentication of your identity. In web3, your wallet is your identity. Generate your signature data in [the block browser](https://polkadot.js.org/apps/), and then add your signature information in the API request header to authenticate your identity. As shown in the figure below:

![sign.png](../picture/sign.png)

The authentication information you need to add in the header:
| Name | Description | Example |
| ---- | ----------- | ------- |
| Account | wallet account | cX... |
| Message | signed message | 123 |
| Signature | signature | 0x... |

## Signature expiration time

The signature expiration time is set by the user. The method is to fill in the expiration time in the message for signing. The gateway will determine whether the current time has exceeded the time in the message. If it has exceeded, the signature expires. The gateway supports setting two time formats:
 - Unix timestamp: seconds and milliseconds
 - Formatted time: 2024-08-13 08:00:00