# Objective

In this tutorial, we will walk through a full dApp development experience, write out both the smart contract, based on [Ink!](https://use.ink/) and the frontend, based on [React](https://react.dev/). Then we will deploy the smart contract on the CESS development node/chain, and have the frontend interact with it.

The application that we will develop is a Proof of Existence (PoE) dApp. It is used to prove that: 1) one is the owner of a certain file, and 2) the file has exactly the same content as it was when being published on-chain in the past (think to prove the content of a contract has not been tampered with).

We will use a simple logic to do this. We extract the first 64kB of the file content, pass it as the input to a hash function to get its digest, and then check if the digest has been recorded on-chain before. Users can also send a transaction on-clain to claim that he own the file with that digest.

{% hint style="success" %}

The complete source code of this tutorial can be cloned/seen at:<br/>
https://github.com/hkwtf/cess-course/tree/main/examples/poe-ink

{% endhint %}

We will first code the smart contract (Ink!) side, and then the frontend side. Let's jump right in.

# Smart Contract (Ink!)

## Prequisite

This section the same prerequsites as the tutorial [Deploy an Ink! Smart Contract](./deploy-sc-ink.md#prerequisite). Please follow that section and install all required components: Rust and `cargo-contract`.

## Start

1. Let's start by building the directory structure

    ```bash
    mkdir poe-ink
    cd poe-ink
    cargo contract new contract
    ```

    The last command will create an Ink! contract project skeleton.

    Inside the directory there are three files.

    ```
    - .gitignore: files to ignore when commiting to git
    - Cargo.toml: This is a Rust project, so there is a `Cargo.toml` file for the project specification.
    - lib.rs: the actual contract code and unit test code.
    ```

    The most interesting file is `lib.rs`. Please skim through it to get an idea. It is a simple contract that read and flip a boolean value.

2. Let's build and test the contract project.

    ```bash
    cd contract          # In case you haven't get inside the contract dir.
    cargo contract build # This command builds the contract project.
    cargo test           # This command run the unit test code starting at the `mod tests` line in the code.
    ```

    Running the `cargo contract build` yields three files:

    - `contract.wasm`: the contract code
    - `contract.json`: the contract metadata
    - `contract.contract`: the contract code and metadata

    The frontend (see next section) will need to read `contract.json` to know the API of the contract. We will use `contract.contract` to instantiate the contract on-chain.

3. Open `lib.rs`, let's remove everything and only keep the top level structure. So we have:

    ```rs
    #![cfg_attr(not(feature = "std"), no_std, no_main)]

    #[ink::contract]
    mod contract {
      // we will fill up the code here next
    }
    ```

4. In smart contract development, two of the keys are how the storage data structure look like, and what events it emit. So thinking about the PoE functionality we will need a map from user addresss to an array of files, indicated by the hash digest, they owned, and a reverse map from the hash digest to its owner. Let's also specify that a file can only be owned by at most one user only. The following storage structure support it:

    ```rs
    mod contract {
      // The following two lines are added to import the support of vector `Vec` and `Mapping` data structure.
      use ink::prelude::{vec, vec::Vec};
      use ink::storage::Mapping;

      #[ink(storage)]
      pub struct Contract {
        /// Mapping from AccountId to hash of files the user owned
        users: Mapping<AccountId, Vec<Hash>>,
        /// Mapping from the file hash to its owner
        files: Mapping<Hash, AccountId>,
      }
    }
    ```

    We use `#[ink(storage)]` attribute macro to tell Rust compiler this is the smart contract storage, and the compiler will further process the storage specification here. [Check here](https://github.com/paritytech/ink#ink-macros--attributes-overview) to learn more about the macros ink! used.

5. Now we want our smart contract to emit events when a user successfully claim the file ownership. Let's also allow the user to forfeit the claim in future. So the two events are:

    ```rs
    mod contract {
      // ... below the storage data structure code

      #[ink(event)]
      pub struct Claimed {
        #[ink(topic)]
        owner: AccountId,
        #[ink(topic)]
        file: Hash,
      }

      #[ink(event)]
      pub struct Forfeited {
        #[ink(topic)]
        owner: AccountId,
        #[ink(topic)]
        file: Hash,
      }
    }
    ```

    We specify two events:
    - **Claimed**: when the event is emitted, the associated user and the file digest are also emitted.
    - **Forfeit**: same as above, the associated user and the file digest are also emitted.

6. Let work on the core logic of the smart contract

    ```rs
    mod contract {
      // ... below the event code

      impl Contract {
        /// Constructor to initialize the contract
        #[ink(constructor)]
        pub fn new() -> Self {
          let users = Mapping::default();
          let files = Mapping::default();
          Self { users, files }
        }

        #[ink(message)]
        pub fn owned_files(&self) -> Vec<Hash> {
          let from = self.env().caller();
          self.users.get(from).unwrap_or(Vec::<Hash>::new())
        }

        #[ink(message)]
        pub fn has_claimed(&self, file: Hash) -> bool {
          match self.files.get(file) {
            Some(_) => true,
            None => false,
          }
        }
      }
    }
    ```

    Here we have defined three methods:

    - `fn new()`: the smart contract constructor. This is the function that will be called when the smart contract is instantiated. It is marked with `#[ink(constructor)]` right above the function.

    - `fn owned_files(&self)`: This function returns a vector, Rust way of saying an array, of Hash. It first retrieves the caller of the smart contract, uses it as the key to retrieve its value inside the **`users`** storage map, and returns the value. If no value is found by the key, an empty vector is returned.

        Notice this function doesn't take an `AccountId` parameter in, so the caller can only checks its own file ownership but not others.

    - `fn has_claimed(&self, file: Hash)`: The function returns a boolean, indicating if the file, specified by the hash, has been claimed by some user. We read from the smart contract storage **`files`**, using the file hash as the key and see if an owner can be retrieved. If yes, the file is claimed and the function returns true. Otherwise, returns false.

        Notice we haven't specified what hash function to use and how a file is converted to its hash digest. This is actually the job of the frontend so we will take care of this in the next section.

7. Now let's work on the core logic of `fn claim()`, a user claiming the ownership of a certain file. The overall logic is that:

    - we first check the file hasn't been claimed before.
    - we update the storage `files` and `users` to indicate the caller has claimed the file ownership
    - emit an event that the file has been claimed so blockchain listeners know about this.

    The following code entails the above logics:

    ```rs
    mod contract {
      // ...previous code
      impl Contract {
        // ...previous code

        /// A message to claim the ownership of the file hash
        #[ink(message)]
        pub fn claim(&mut self, file: Hash) -> Result<()> {
          // Get the caller
          let from = self.env().caller();

          // Check the hash hasn't been claimed yet
          if self.files.contains(file) {
            return Err(Error::AlreadyClaimed);
          }

          // Claim the file hash ownership with two write ops
          // Update the `users` storage. If a vector is retrieved, we push the hash digest into
          //   the vector. Otherwise we create a new vector with the hash digest element inside.
          match self.users.get(from) {
            Some(mut files) => {
              // A user entry has already been built
              files.push(file);
              self.users.insert(from, &files);
            }
            None => {
              // A user entry hasn't been built, so building one here
              self.users.insert(from, &vec![file]);
            }
          }

          // Update the `files` storage
          self.files.insert(file, &from);

          // Emit an event
          Self::env().emit_event(Claimed { owner: from, file });

          Ok(())
        }
      }
    }
    ```

8. You may have noticed the return value type of `fn claim()` look different from the two previous functions `fn owned_files()` and `fn has_claimed()`. `fn owned_files()` and `fn has_claimed()` are view functions, and they only read the contract status/storage but doesn't alter it. `fn claim()` on the other hand is a state modifying function. It returns a [`Result` enum type in Rust](https://doc.rust-lang.org/std/result/enum.Result.html) to indicate whether the function successfully change the state by returning `Ok(())`, or an error occurs and state is not changed at all `Err(error value here)`. In fact, the result is a short hand to a longer form that we will see later in the code.

    Now, let's define the longer form of Result and the error values. Two error values are going to emitted in our contract, when a user try to claim a file that has already been claimed, `AlreadyClaimed`; and when users trying to forfeit the file ownership of a file they don't own, `NotOwner`.

    ```rs
    mod contract {
      // ... prev code

      // Beware this part of the code is OUTSIDE of `impl Contract {}`, unlike the claim() function above.

      // Result type used in `fn claim()` is a short form.
      pub type Result<T> = core::result::Result<T, Error>;

      // These are two error values returned in our contract
      #[derive(Debug, PartialEq, Eq, scale::Encode, scale::Decode)]
      #[cfg_attr(feature = "std", derive(scale_info::TypeInfo))]
      pub enum Error {
        /// File with the specified hash has been claimed by another user
        AlreadyClaimed,
        /// Caller doesn't own the file with the specified hash
        NotOwner,
      }

      impl Contract {
        // ... prev code
      }
    }
    ```

9. Regarding the `fn forfeit()` function, it is basically reversing the above `fn claim()` operation in the above step.

    - we first check the caller actually own the file. If not we return an error
    - we update the **`users`** and **`files`** storage to remove the file hash digest
    - emit an event about this new state.

    ```rs
    mod contract {
      // ...previous code
      impl Contract {
        // ...previous code
        #[ink(message)]
        pub fn forfeit(&mut self, file: Hash) -> Result<()> {
          let from = self.env().caller();

          // Check if the file is owned by the caller. If not, returns `Error::NotOwner`.
          match self.files.get(file) {
            Some(owner) => {
              if owner != from {
                return Err(Error::NotOwner);
              }
            }
            None => {
              return Err(Error::NotOwner);
            }
          }

          // Confirmed the caller is the file owner. Update the two storage `users` and `files`.
          let mut files = self.users.get(from).unwrap_or(vec![]);
          for idx in 0..files.len() {
            if files[idx] == file {
              files.swap_remove(idx);
              self.users.insert(from, &files);
            }
          }

          self.files.remove(file);

          // Emit an event
          Self::env().emit_event(Forfeited { owner: from, file });

          Ok(())
        }
      }
    }
    ```

10. By this point, you have completed all the core logic of the smart contract. Compile the contract with `cargo contract build` to ensure it builds. You can always refer back to the [**full source code**](https://github.com/hkwtf/cess-course/blob/main/examples/poe-ink/contract/lib.rs). **Congratulation**!

    Next step before heading to the frontend.

    - The full source code contains also the unit test code, the code block inside `mod tests { ... }`. We won't go over them here, but they are pretty self-explanatory. Please take a look. You can run them by `cargo test`. [Check here](https://use.ink/basics/contract-testing) to learn more about contract testing.
    - After the compilation, try to [deploy the contract on your local cess dev chain](./deploy-sc-ink.md) and interact with the contract to test it out.

# Frontend


# Deployment


# References
