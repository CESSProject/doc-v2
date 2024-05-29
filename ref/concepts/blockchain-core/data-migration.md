When the chain needs to be upgraded after its runtime logic is modified, it may involve changes in the data storage structure. In this case, users need to define a data migration logic to store the old data in the new structure.

# Storage Migration

Storage migrations are user-defined, one-time functions that allow developers to rework existing storage in order to convert it to conform to updated expectations. For instance, imagine a runtime upgrade that changes the data type used to represent user balances from an unsigned integer to a signed integer - in this case, the storage migration would read the existing value as an unsigned integer and write back an updated value that has been converted to a signed integer. Failure to perform a storage migration when needed will result in the runtime execution engine misinterpreting the storage values that represent the runtime state and lead to undefined behavior.

# Framework Implementation

FRAME storage migrations are implemented by way of the [`OnRuntimeUpgrade` trait](https://paritytech.github.io/substrate/master/frame_support/traits/trait.OnRuntimeUpgrade.html), which specifies a function `on_runtime_upgrade()`. This function provides a hook that allows runtime developers to specify logic that will run immediately after a runtime upgrade but before any `on_initialize()` function has executed.

# Testing Migration

It is important to test storage migrations. In the `OnRuntimeUpgrade` trait, two functions `pre_upgrade()` and `post_upgrade()` are defined for the use of testing.

For a more specific approach, please refer to the official [Substrate migration examples](https://github.com/paritytech/substrate/pulls?q=is%3Apr+is%3Aclosed+label%3AE0-runtime_migration).
