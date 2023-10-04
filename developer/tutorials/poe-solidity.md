# Objective

In this tutorial, we will walk through the whole process of building a full dApp, writing both the smart contract in Solidity and the frontend in React. Again, we will be building the Proof of Existance App. For those who are not familiar with what Proof of Existance on blockchain is, or haven't gone through the previous tutorial. Please take a look at the Proof of Existance [background introduction](./poe-ink.md#objective).

{% hint style="success" %}

The complete source code of this tutorial can be seen at our [`cess-course` repository](https://github.com/CESSProject/cess-course).

- [Smart Contract](https://github.com/CESSProject/cess-course/blob/main/examples/hardhat/contracts/ProofOfExistence.sol)
- [Front end](https://github.com/CESSProject/cess-course/blob/main/examples/frontend/src/ProofOfExistenceSolidity.js)

{%endhint}

Let's first look at the smart contract (Solidity) side and then the front-end side.

# Smart Contract (Solidity)

## Prerequisites

We will use [**Hardhat**](https://hardhat.org/) as the smart contract development toolchain. So, let's install and initialize `hardhat` library.

```bash
mkdir hardhat
cd hardhat
pnpm dlx hardhat init
```

You will be asked a series of questions on how the project be configured.

```
✔ What do you want to do? · Create a TypeScript project
✔ Hardhat project root: · (your chosen dir)
✔ Do you want to add a .gitignore? (Y/n) · y
✔ Do you want to install this sample project's dependencies with npm (hardhat @nomicfoundation/hardhat-toolbox)? (Y/n) · y
```

![insert-image]()

Hardhat is default to be using `npm` package manager, but feel free to use your your own preferred package manager.

Now let's also use [**`hardhat-deploy`**](https://github.com/wighawag/hardhat-deploy) library to help deploy and manage deployed smart contracts.

```bash
pnpm install -D hardhat-deploy
```

By default, you should have a **Lock.sol** smart contract inside `hardhat/conttracts`. Check that everything works by running `pnpm hardhat test` and see all test cases pass.

If you have any issue, refer back to the [`hardhat` directory](https://github.com/CESSProject/cess-course/tree/main/examples/hardhat) and its configuration ([`package.json`](https://github.com/CESSProject/cess-course/blob/main/examples/hardhat/package.json) and [`hardhat.config.ts`](https://github.com/CESSProject/cess-course/blob/main/examples/hardhat/hardhat.config.ts)).

## Development

1. Let's configure `hardhat.config.ts` so it can deploy smart contract to default hardhat network, a locally running hardhat node, and a locally running cess node.

    Please use the following hardhat config in `hardhat.config.ts`

    ```ts
    const config: HardhatUserConfig = {
      solidity: "0.8.19",
      defaultNetwork: "hardhat",
      namedAccounts: {
        deployer: {
          default: 0,
        },
        beneficiary: {
          default: 1,
        },
      },
      networks: {
        hardhat: {
          // issue: https://github.com/sc-forks/solidity-coverage/issues/652,
          // refer to : https://github.com/sc-forks/solidity-coverage/issues/652#issuecomment-896330136
          initialBaseFeePerGas: 0
        },
        localhost: {
          url: "http://localhost:8545",
          accounts: ["0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"],
        },
        "cess-local": {
          url: "http://localhost:9944", // RPC endpoint of CESS testnet
          chainId: 11330,
          // private key of `//Alice` from Substrate
          accounts: ["0xe5be9a5092b81bca64be81d212e7f2f9eba183bb7a90954f7b76361f6edb5c0a"],
        }
      }
    };
    ```

    With this config and `hardhat-deploy`, we can deploy smart contract to our locally running CESS node by `pnpm hardhat deploy --network cess-local`.

2. Now for the smart contract, we want to have the following methods:

    - `claim(bytes32 hash)`: a method for the caller to claim ownership of a file with the specified hash.
    - `forfeit(bytes32 hash)`: a method for the caller to forfeit ownership of a file with the specified hash.
    - `ownedFiles()`:  retrieving all the file hashes owned by the user.
    - `hasClaimed(hash)`: check given a file hash, has it been claimed yet.

    ```solidity
    // SPDX-License-Identifier: UNLICENSED
    pragma solidity ^0.8.19;

    contract ProofOfExistence {
      mapping(bytes32 => address) public files;
      mapping(address => bytes32[]) public users;

      event Claimed(address indexed owner, bytes32 indexed file);
      event Forfeited(address indexed owner, bytes32 indexed file);

      error NotFileOwner();
      error FileAlreadyClaimed();

      modifier isOwner(bytes32 hash) {
        address from = msg.sender;
        if (files[hash] != from) revert NotFileOwner();
        _;
      }

      modifier notClaimed(bytes32 hash) {
        address from = msg.sender;
        if (files[hash] != address(0)) revert FileAlreadyClaimed();
        _;
      }

      function hasClaimed(bytes32 hash) public view returns (bool) {
        address owner = files[hash];
        return (owner != address(0));
      }

      function ownedFiles() public view returns (bytes32[] memory) {
        address from = msg.sender;
        return users[from];
      }

      function claim(bytes32 hash) public notClaimed(hash) returns (bool) {
        address from = msg.sender;

        // update storage files
        files[hash] = from;

        // udpate storage users
        bytes32[] storage userFiles = users[from];
        userFiles.push(hash);

        emit Claimed(from, hash);
        return true;
      }

      function forfeit(bytes32 hash) public isOwner(hash) returns (bool) {
        address from = msg.sender;

        // update storage files
        delete files[hash];

        // locate the index of the file going to be deleted.
        bytes32[] storage userFiles = users[from];
        uint32 delIdx = 0;
        for (uint32 i = 0; i < userFiles.length; i++) {
          if (userFiles[i] == hash) {
            delIdx = i;
            break;
          }
        }
        // update storage users by swap-delete
        if (delIdx != userFiles.length - 1) {
          userFiles[delIdx] = userFiles[userFiles.length - 1];
        }
        // delete
        userFiles.pop();

        emit Forfeited(from, hash);
        return true;
      }
    }
    ```
