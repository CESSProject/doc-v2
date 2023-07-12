# Runtime Upgrade

Forkless upgrade is a defining feature in Substrate used for blockchain development. The ability to update Runtime logic without forking the code base enables the blockchain to evolve and improve over time. This functionality can be achieved by defining Runtime  (runtime WebAssembly blob) as part of the state of the blockchain.

Because Substrate runtime is a part of the blockchain state, network maintainers can leverage the functionality of blockchain to achieve trustless, decentralized consensus and enhance Runtime security.

In the runtime development framework [**FRAME**](https://docs.substrate.io/learn/runtime-development/#frame), the system library uses `set_code()` calls to upgrade the runtime definition. As a result, runtime can be upgraded without shutting down the nodes or interrupting the blockchain operations. If the runtime upgrade requires changing the existing state, it is likely that [data migration](data-migration.md) is required.

# Runtime Version

During the process of runtime version control, compiling nodes will generate both platform native binaries and WebAssembly binaries. You can control which binaries to use at different points in the block production process with different command-line policy options. The components that choose the runtime execution environment to communicate with are called executors. Although you can override the default execution strategy for custom scenarios, in most cases, the execution program selects the appropriate binary to use by evaluating the following information for both native and WebAssembly runtime binaries:

- [`spec_name`]()
- [`spec_version`]()
- [`authoring_version`]()

To provide these information to the executor, the runtime has the following [`RuntimeVersion`](https://paritytech.github.io/substrate/master/sc_cli/struct.RuntimeVersion.html) struct:

```rust
pub const VERSION: RuntimeVersion = RuntimeVersion {
  spec_name: create_runtime_str!("cess-node"),
  impl_name: create_runtime_str!("cess-node"),
  authoring_version: 1,
  spec_version: 100,
  impl_version: 1,
  apis: RUNTIME_API_VERSIONS,
  transaction_version: 1,
  state_version: 1,
};
```

# Forkless Runtime Upgrade

Traditional blockchains require hard-forking when upgrading their chain's state transition logics. Hard fork requires all node operators to stop their nodes and manually upgrade to the latest executable. For distributed production networks, coordinating hard fork upgrades can be a complex process.

`RuntimeVersion` control attribute enables Substrate-based blockchains to upgrade runtime logic in real-time without causing network forks.

In order to perform a forkless runtime upgrade, Substrate use the existing Runtime logic to update the Wasm runtime stored on the blockchain to a new consensus breaking version with new logic. As part of the consensus process, this upgrade is pushed to all full nodes on the network.
