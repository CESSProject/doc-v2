CESS blockchain supports two types of smart contracts:

- WebAssembly contract, such as [**ink!**](https://use.ink/)
- EVM compatible contracts, such as [**Solidity**](https://soliditylang.org/)

# Smart Contract Account

The [**Contracts Pallet**](https://paritytech.github.io/substrate/master/pallet_contracts) of Runtime extends the account system of Currency trait, for it to have smart contract functionality. You can use these smart contract accounts to instantiate contracts and call other contracts and non contract accounts.

The smart contract code is stored in the cache and can be retrieved by its hash. This design allows multiple smart contracts to be instantiated from the same hash without copying code each time.

When a user interacts with a smart contract by calling one of its functions, the associated smart contract code is retrieved and then get executed. Calling the smart contract function may result in:

- Change the storage associated with this smart contract account.
- Change the storage associated with a non contract account.
- Instantiate a new smart contract.
- Call another smart contract.

If the smart contract account is deleted, its associated code and storage will also be deleted.

# Contract Execution and Gas

Calling a smart contract requires paying a gas fee. The transaction sender must specify a gas limit for each call. Regardless of the execution result, unused gas will be returned after the call.

After the specified gas limit is reached, all calls and related status changes (including transfers) will be rolled back only at the currently called contract layer. For example, if contract A calls contract B, and there is not enough gas when B executes, all B calls will be rolled back. If A can handle errors correctly, then other calls and state changes of A can still be persisted.
