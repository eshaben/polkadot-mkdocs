---
title: Pay Transaction Fees with Different Tokens
description: Learn how to send a DOT transfer transaction while paying the fees using a different token on Polkadot Hub using multiple SDKs.
categories:
- Chain Interactions
- Tooling
url: https://docs.polkadot.com/chain-interactions/send-transactions/pay-fees-with-different-tokens/
word_count: 2839
token_estimate: 6065
version_hash: sha256:32885264e726cc5de1cf98a503bfa7185ea6729b6f31d92f30949e9683d019c3
last_updated: '2026-04-15T03:51:44+00:00'
---

# Send a Transaction While Paying the Fee with a Different Token

## Introduction

Polkadot Hub allows users to pay transaction fees using alternative tokens instead of the native token. This tutorial demonstrates how to send a DOT transfer transaction while paying the fees using USDT on Polkadot Hub.

You can follow this tutorial using [Polkadot-API (PAPI)](/polkadot-mkdocs/reference/tools/papi/), [Polkadot.js API](/polkadot-mkdocs/reference/tools/polkadot-js-api/), or [Subxt](/polkadot-mkdocs/reference/tools/subxt/). Select your preferred SDK in the code tabs below.

!!! warning "Polkadot.js API Maintenance Mode"
    The Polkadot.js API is no longer actively developed. New projects should use [Polkadot-API (PAPI)](/polkadot-mkdocs/reference/tools/papi/) or [Dedot](/polkadot-mkdocs/reference/tools/dedot/) as actively maintained alternatives.

## Prerequisites

Before starting, ensure you have the following installed:

- [Chopsticks](/polkadot-mkdocs/reference/tools/chopsticks/) — to fork Polkadot Hub locally
- Your preferred SDK:
    - [Polkadot-API (PAPI)](/polkadot-mkdocs/reference/tools/papi/) (TypeScript)
    - [Polkadot.js API](/polkadot-mkdocs/reference/tools/polkadot-js-api/) (JavaScript)
    - [Subxt](/polkadot-mkdocs/reference/tools/subxt/) (Rust)

## Local Polkadot Hub Setup

Fork the Polkadot Hub locally using [Chopsticks](/polkadot-mkdocs/reference/tools/chopsticks/):

```bash
chopsticks -c polkadot-asset-hub
```

This command forks the Polkadot Hub chain, making it available at `ws://localhost:8000`. When running `polkadot-asset-hub`, you use the Polkadot Hub fork with the configuration specified in the [`polkadot-asset-hub.yml`](https://github.com/AcalaNetwork/chopsticks/blob/master/configs/polkadot-asset-hub.yml) file. This configuration defines Alice's account with USDT assets. If you want to use a different chain, ensure the account you use has the necessary assets.

## Set Up Your Project

=== "PAPI"

    1. Create a new directory and initialize the project:

        ```bash
        mkdir fee-payment-tutorial && cd fee-payment-tutorial
        ```

    2. Initialize the project:

        ```bash
        npm init -y
        ```

    3. Install dev dependencies:

        ```bash
        npm install --save-dev @types/node@^22.12.0 ts-node@^10.9.2 typescript@^5.7.3
        ```

    4. Install dependencies:

        ```bash
        npm install --save @polkadot-labs/hdkd@0.0.27 @polkadot-labs/hdkd-helpers@0.0.28 polkadot-api@1.23.3
        ```

    5. Create TypeScript configuration:

        ```bash
        npx tsc --init && npm pkg set type=module
        ```

    6. Generate Polkadot API types for Polkadot Hub:

        ```bash
        npx papi add assetHub -n polkadot_asset_hub
        ```

    7. Create a new file called `fee-payment-transaction.ts`:

        ```bash
        touch fee-payment-transaction.ts
        ```

=== "Polkadot.js"

    1. Create a new directory and initialize the project:

        ```bash
        mkdir fee-payment-tutorial && cd fee-payment-tutorial
        ```

    2. Initialize the project:

        ```bash
        npm init -y && npm pkg set type=module
        ```

    3. Install dependencies:

        ```bash
        npm install @polkadot/api@16.5.6
        ```

    4. Create a new file called `fee-payment-transaction.js`:

        ```bash
        touch fee-payment-transaction.js
        ```

=== "Subxt"

    1. Create a new Rust project:

        ```bash
        cargo new subxt-fee-payment-example && cd subxt-fee-payment-example
        ```

    2. Install the Subxt CLI to download chain metadata:

        ```bash
        cargo install subxt-cli@0.50.0
        ```

    3. Download Polkadot Hub metadata from the local Chopsticks fork:

        ```bash
        mkdir metadata && \
        subxt metadata --url ws://localhost:8000 -o metadata/asset_hub.scale
        ```

        !!! note
            Ensure your Chopsticks fork is running at `ws://localhost:8000` before downloading the metadata.

    4. Update `Cargo.toml` with the required dependencies:

        ```toml title="Cargo.toml"
        [package]
        name = "subxt-fee-payment-example"
        version = "0.1.0"
        edition = "2021"

        [[bin]]
        name = "fee_payment_transaction"
        path = "src/bin/fee_payment_transaction.rs"

        [dependencies]
        codec = { package = "parity-scale-codec", version = "3", features = ["derive"] }
        subxt = { version = "0.50.0", features = ["jsonrpsee", "native"] }
        subxt-signer = { version = "0.50.0", features = ["sr25519"] }
        tokio = { version = "1.44.2", features = ["macros", "rt-multi-thread"] }
        ```

    5. Create the source file:

        ```bash
        mkdir -p src/bin && touch src/bin/fee_payment_transaction.rs
        ```

## Implementation

The following sections cover how to set up imports and constants, create a transaction signer, connect to Polkadot Hub, and send a DOT transfer transaction while paying fees in USDT.

### Import Dependencies and Define Constants

Set up the required imports and define the target address, transfer amount, and USDT asset ID for your transaction:

=== "PAPI"

    ```typescript title="fee-payment-transaction.ts"
    import { sr25519CreateDerive } from "@polkadot-labs/hdkd";
    import {
      DEV_PHRASE,
      entropyToMiniSecret,
      mnemonicToEntropy,
    } from "@polkadot-labs/hdkd-helpers";
    import { getPolkadotSigner } from "polkadot-api/signer";
    import { createClient } from "polkadot-api";
    import { assetHub } from "@polkadot-api/descriptors";
    import { withPolkadotSdkCompat } from "polkadot-api/polkadot-sdk-compat";
    import { getWsProvider } from "polkadot-api/ws-provider/node";
    import { MultiAddress } from "@polkadot-api/descriptors";

    const TARGET_ADDRESS = "14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3"; // Bob's address
    const TRANSFER_AMOUNT = 3_000_000_000n; // 3 DOT
    const USDT_ASSET_ID = 1984;
    ```

=== "Polkadot.js"

    ```javascript title="fee-payment-transaction.js"
    import { ApiPromise, WsProvider } from '@polkadot/api';
    import { Keyring } from '@polkadot/keyring';
    import { cryptoWaitReady } from '@polkadot/util-crypto';

    const TARGET_ADDRESS = '14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3'; // Bob's address
    const TRANSFER_AMOUNT = 3_000_000_000n; // 3 DOT
    const USDT_ASSET_ID = 1984;
    ```

=== "Subxt"

    In Subxt, you first generate types from the chain metadata using the `#[subxt::subxt()]` macro. The `derive_for_type` attribute ensures the `Location` type implements the traits needed for encoding. You also define a custom `AssetHubConfig` where `type AssetId = Location`, which enables specifying an XCM location as the fee payment asset:

    ```rust title="src/bin/fee_payment_transaction.rs"
    use std::str::FromStr;
    use subxt::config::{Config, DefaultExtrinsicParamsBuilder, DefaultTransactionExtensions, PolkadotConfig};
    use subxt::utils::AccountId32;
    use subxt::{OnlineClient, SubstrateConfig};

    // Generate types from the Polkadot Hub metadata with Location trait derives
    #[subxt::subxt(
        runtime_metadata_path = "metadata/asset_hub.scale",
        derive_for_type(
            path = "staging_xcm::v5::location::Location",
            derive = "Clone, Eq, PartialEq, codec::Encode",
            recursive
        )
    )]
    pub mod asset_hub {}

    // Import XCM location types from the generated metadata module
    use asset_hub::runtime_types::staging_xcm::v5::{
        junction::Junction,
        junctions::Junctions,
        location::Location,
    };

    // Define a custom config where AssetId is an XCM Location
    #[derive(Debug, Default, Clone)]
    pub struct AssetHubConfig;

    impl Config for AssetHubConfig {
        type AccountId = <PolkadotConfig as Config>::AccountId;
        type Address = <PolkadotConfig as Config>::Address;
        type Signature = <PolkadotConfig as Config>::Signature;
        type Hasher = <PolkadotConfig as Config>::Hasher;
        type Header = <SubstrateConfig as Config>::Header;
        type TransactionExtensions = DefaultTransactionExtensions<AssetHubConfig>;
        type AssetId = Location;
    }

    const POLKADOT_HUB_RPC: &str = "ws://localhost:8000";
    const TARGET_ADDRESS: &str = "14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3";
    const TRANSFER_AMOUNT: u128 = 3_000_000_000; // 3 DOT
    const USDT_ASSET_ID: u128 = 1984;
    ```

### Create a Signer and Connect

Create a signer using Alice's development account and connect to the local Polkadot Hub:

=== "PAPI"

    ```typescript title="fee-payment-transaction.ts"
    const createSigner = async () => {
      const entropy = mnemonicToEntropy(DEV_PHRASE);
      const miniSecret = entropyToMiniSecret(entropy);
      const derive = sr25519CreateDerive(miniSecret);
      const hdkdKeyPair = derive("//Alice");
      const polkadotSigner = getPolkadotSigner(
        hdkdKeyPair.publicKey,
        "Sr25519",
        hdkdKeyPair.sign
      );
      return polkadotSigner;
    };

    const client = createClient(
      withPolkadotSdkCompat(
        getWsProvider("ws://localhost:8000") // Chopsticks Polkadot Hub
      )
    );

    const api = client.getTypedApi(assetHub);
    ```

=== "Polkadot.js"

    The `cryptoWaitReady()` call ensures the underlying WASM cryptographic libraries are initialized before creating the keyring:

    ```javascript title="fee-payment-transaction.js"
    async function main());
      const alice = keyring.addFromUri('//Alice');

      // Connect to the local Chopsticks Polkadot Hub fork
      const wsProvider = new WsProvider('ws://localhost:8000');
      const api = await ApiPromise.create({ provider: wsProvider });
      console.log('Connected to Polkadot Hub (Chopsticks fork)');
    ```

=== "Subxt"

    Notice that the `OnlineClient` is parameterized with `AssetHubConfig` instead of the default `PolkadotConfig`:

    ```rust title="src/bin/fee_payment_transaction.rs"
    #[tokio::main]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
        // Connect to the local Chopsticks Polkadot Hub fork
        let api = OnlineClient::<AssetHubConfig>::from_url(POLKADOT_HUB_RPC).await?;
        println!("Connected to Polkadot Hub (Chopsticks fork)");

        // Anchor to the current block
        let at_block = api.at_current_block().await?;

        // Create Alice's dev keypair
        let alice = subxt_signer::sr25519::dev::alice();
        println!("Sender (Alice): {}", AccountId32::from(alice.public_key()));
    ```

### Create the Transaction

Create a standard DOT transfer transaction that sends 3 DOT to Bob's address while keeping Alice's account alive:

=== "PAPI"

    ```typescript title="fee-payment-transaction.ts"
    const tx = api.tx.Balances.transfer_keep_alive({
      dest: MultiAddress.Id(TARGET_ADDRESS),
      value: BigInt(TRANSFER_AMOUNT),
    });
    ```

=== "Polkadot.js"

    ```javascript title="fee-payment-transaction.js"
      // Create the transfer transaction
      const tx = api.tx.balances.transferKeepAlive(TARGET_ADDRESS, TRANSFER_AMOUNT);
    ```

=== "Subxt"

    ```rust title="src/bin/fee_payment_transaction.rs"
        // Create the balance transfer transaction
        let dest = AccountId32::from_str(TARGET_ADDRESS)?;
        let tx = asset_hub::transactions()
            .balances()
            .transfer_keep_alive(dest.into(), TRANSFER_AMOUNT);
    ```

### Sign and Submit with Alternative Fee Payment

The key part of this tutorial is specifying an alternative asset for fee payment. The USDT asset is identified using the XCM location format, where `PalletInstance(50)` refers to the Assets pallet and `GeneralIndex(1984)` identifies the USDT asset on Polkadot Hub:

=== "PAPI"

    In PAPI, you specify the alternative asset through the `asset` parameter in the `signAndSubmit` options:

    ```typescript title="fee-payment-transaction.ts"
    const signer = await createSigner();

    const result = await tx.signAndSubmit(signer, {
      asset: {
        parents: 0,
        interior: {
          type: "X2",
          value: [
            { type: "PalletInstance", value: 50 },
            { type: "GeneralIndex", value: BigInt(USDT_ASSET_ID) },
          ],
        },
      },
    });

    const { txHash, ok, block, events } = result;
    console.log(`Tx finalized: ${txHash} (ok=${ok})`);
    console.log(`Block: #${block.number} ${block.hash} [tx index ${block.index}]`);

    console.log("Events:");
    for (const ev of events)`);
    }

    process.exit(0);
    ```

=== "Polkadot.js"

    In Polkadot.js, you define the asset as an XCM multi-location object and pass it as the `assetId` option to `signAndSend`:

    ```javascript title="fee-payment-transaction.js"
      // Define the USDT asset as an XCM multi-location for fee payment
      const assetId = {
        parents: 0,
        interior: {
          X2: [{ PalletInstance: 50 }, { GeneralIndex: USDT_ASSET_ID }],
        },
      };

      // Sign and send the transaction, paying fees with USDT
      console.log('Signing and submitting transaction...');
      await new Promise((resolve, reject) => {
        let unsubscribe;
        tx
          .signAndSend(
            alice,
            { assetId },
            ({ status, events, txHash, dispatchError }) => {
              if (status.isFinalized)`
                );
                console.log(`Transaction hash: ${txHash.toHex()}`);

                // Display all events
                console.log('\nEvents:');
                events.forEach(({ event }) => {
                  console.log(`  ${event.section}.${event.method}`);
                });

                if (unsubscribe)

                if (dispatchError) = decoded;
                    reject(new Error(`${section}.${name}: ${docs.join(' ')}`));
                  } else {
                    reject(new Error(dispatchError.toString()));
                  }
                } else {
                  resolve();
                }
              }
            }
          )
          .then((unsub) => {
            unsubscribe = unsub;
          })
          .catch((error) => {
            if (unsubscribe)
            reject(error);
          });
      });
    ```

=== "Subxt"

    In Subxt, you use `DefaultExtrinsicParamsBuilder` with `tip_of(0, asset_location)` to specify the fee asset. The first argument is the tip amount (0), and the second is the XCM `Location`. Instead of calling `sign_and_submit_then_watch_default`, you pass the custom `tx_params` to `sign_and_submit_then_watch`:

    ```rust title="src/bin/fee_payment_transaction.rs"
        // Define the USDT asset location in XCM format
        let asset_location = Location {
            parents: 0,
            interior: Junctions::X2([
                Junction::PalletInstance(50),
                Junction::GeneralIndex(USDT_ASSET_ID),
            ]),
        };

        // Build transaction params to pay fees with the alternative asset
        let tx_params = DefaultExtrinsicParamsBuilder::<AssetHubConfig>::new()
            .tip_of(0, asset_location)
            .build();

        // Sign, submit, and watch for finalization
        println!("Signing and submitting transaction...");
        let progress = at_block
            .transactions()
            .sign_and_submit_then_watch(&tx, &alice, tx_params)
            .await?;

        let in_block = progress.wait_for_finalized().await?;
        let block_hash = in_block.block_hash();
        let events = in_block.wait_for_success().await?;

        // Display transaction results
        println!("\nTransaction finalized in block: {:?}", block_hash);

        println!("\nEvents:");
        for event in events.iter().{}",
                event.pallet_name(),
                event.event_name()
            );
        }
    ```

### Full Code

=== "PAPI"

    ??? code "Complete Code"

        ```typescript title="fee-payment-transaction.ts"
        import { sr25519CreateDerive } from "@polkadot-labs/hdkd";
        import {
          DEV_PHRASE,
          entropyToMiniSecret,
          mnemonicToEntropy,
        } from "@polkadot-labs/hdkd-helpers";
        import { getPolkadotSigner } from "polkadot-api/signer";
        import { createClient } from "polkadot-api";
        import { assetHub } from "@polkadot-api/descriptors";
        import { withPolkadotSdkCompat } from "polkadot-api/polkadot-sdk-compat";
        import { getWsProvider } from "polkadot-api/ws-provider/node";
        import { MultiAddress } from "@polkadot-api/descriptors";

        const TARGET_ADDRESS = "14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3"; // Bob's address
        const TRANSFER_AMOUNT = 3_000_000_000n; // 3 DOT
        const USDT_ASSET_ID = 1984;

        const createSigner = async () => {
          const entropy = mnemonicToEntropy(DEV_PHRASE);
          const miniSecret = entropyToMiniSecret(entropy);
          const derive = sr25519CreateDerive(miniSecret);
          const hdkdKeyPair = derive("//Alice");
          const polkadotSigner = getPolkadotSigner(
            hdkdKeyPair.publicKey,
            "Sr25519",
            hdkdKeyPair.sign
          );
          return polkadotSigner;
        };

        const client = createClient(
          withPolkadotSdkCompat(
            getWsProvider("ws://localhost:8000") // Chopsticks Polkadot Hub
          )
        );

        const api = client.getTypedApi(assetHub);

        const tx = api.tx.Balances.transfer_keep_alive({
          dest: MultiAddress.Id(TARGET_ADDRESS),
          value: BigInt(TRANSFER_AMOUNT),
        });

        const signer = await createSigner();

        const result = await tx.signAndSubmit(signer, {
          asset: {
            parents: 0,
            interior: {
              type: "X2",
              value: [
                { type: "PalletInstance", value: 50 },
                { type: "GeneralIndex", value: BigInt(USDT_ASSET_ID) },
              ],
            },
          },
        });

        const { txHash, ok, block, events } = result;
        console.log(`Tx finalized: ${txHash} (ok=${ok})`);
        console.log(`Block: #${block.number} ${block.hash} [tx index ${block.index}]`);

        console.log("Events:");
        for (const ev of events)`);
        }

        process.exit(0);
        ```

=== "Polkadot.js"

    ??? code "Complete Code"

        ```javascript title="fee-payment-transaction.js"
        import { ApiPromise, WsProvider } from '@polkadot/api';
        import { Keyring } from '@polkadot/keyring';
        import { cryptoWaitReady } from '@polkadot/util-crypto';

        const TARGET_ADDRESS = '14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3'; // Bob's address
        const TRANSFER_AMOUNT = 3_000_000_000n; // 3 DOT
        const USDT_ASSET_ID = 1984;

        async function main());
          const alice = keyring.addFromUri('//Alice');

          // Connect to the local Chopsticks Polkadot Hub fork
          const wsProvider = new WsProvider('ws://localhost:8000');
          const api = await ApiPromise.create({ provider: wsProvider });
          console.log('Connected to Polkadot Hub (Chopsticks fork)');

          // Create the transfer transaction
          const tx = api.tx.balances.transferKeepAlive(TARGET_ADDRESS, TRANSFER_AMOUNT);

          // Define the USDT asset as an XCM multi-location for fee payment
          const assetId = {
            parents: 0,
            interior: {
              X2: [{ PalletInstance: 50 }, { GeneralIndex: USDT_ASSET_ID }],
            },
          };

          // Sign and send the transaction, paying fees with USDT
          console.log('Signing and submitting transaction...');
          await new Promise((resolve, reject) => {
            let unsubscribe;
            tx
              .signAndSend(
                alice,
                { assetId },
                ({ status, events, txHash, dispatchError }) => {
                  if (status.isFinalized)`
                    );
                    console.log(`Transaction hash: ${txHash.toHex()}`);

                    // Display all events
                    console.log('\nEvents:');
                    events.forEach(({ event }) => {
                      console.log(`  ${event.section}.${event.method}`);
                    });

                    if (unsubscribe)

                    if (dispatchError) = decoded;
                        reject(new Error(`${section}.${name}: ${docs.join(' ')}`));
                      } else {
                        reject(new Error(dispatchError.toString()));
                      }
                    } else {
                      resolve();
                    }
                  }
                }
              )
              .then((unsub) => {
                unsubscribe = unsub;
              })
              .catch((error) => {
                if (unsubscribe)
                reject(error);
              });
          });

          // Disconnect from the node
          await api.disconnect();
        }

        main().catch(console.error);
        ```

=== "Subxt"

    ??? code "Complete Code"

        ```rust title="src/bin/fee_payment_transaction.rs"
        use std::str::FromStr;
        use subxt::config::{Config, DefaultExtrinsicParamsBuilder, DefaultTransactionExtensions, PolkadotConfig};
        use subxt::utils::AccountId32;
        use subxt::{OnlineClient, SubstrateConfig};

        // Generate types from the Polkadot Hub metadata with Location trait derives
        #[subxt::subxt(
            runtime_metadata_path = "metadata/asset_hub.scale",
            derive_for_type(
                path = "staging_xcm::v5::location::Location",
                derive = "Clone, Eq, PartialEq, codec::Encode",
                recursive
            )
        )]
        pub mod asset_hub {}

        // Import XCM location types from the generated metadata module
        use asset_hub::runtime_types::staging_xcm::v5::{
            junction::Junction,
            junctions::Junctions,
            location::Location,
        };

        // Define a custom config where AssetId is an XCM Location
        #[derive(Debug, Default, Clone)]
        pub struct AssetHubConfig;

        impl Config for AssetHubConfig {
            type AccountId = <PolkadotConfig as Config>::AccountId;
            type Address = <PolkadotConfig as Config>::Address;
            type Signature = <PolkadotConfig as Config>::Signature;
            type Hasher = <PolkadotConfig as Config>::Hasher;
            type Header = <SubstrateConfig as Config>::Header;
            type TransactionExtensions = DefaultTransactionExtensions<AssetHubConfig>;
            type AssetId = Location;
        }

        const POLKADOT_HUB_RPC: &str = "ws://localhost:8000";
        const TARGET_ADDRESS: &str = "14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3";
        const TRANSFER_AMOUNT: u128 = 3_000_000_000; // 3 DOT
        const USDT_ASSET_ID: u128 = 1984;

        #[tokio::main]
        async fn main() -> Result<(), Box<dyn std::error::Error>> {
            // Connect to the local Chopsticks Polkadot Hub fork
            let api = OnlineClient::<AssetHubConfig>::from_url(POLKADOT_HUB_RPC).await?;
            println!("Connected to Polkadot Hub (Chopsticks fork)");

            // Anchor to the current block
            let at_block = api.at_current_block().await?;

            // Create Alice's dev keypair
            let alice = subxt_signer::sr25519::dev::alice();
            println!("Sender (Alice): {}", AccountId32::from(alice.public_key()));

            // Create the balance transfer transaction
            let dest = AccountId32::from_str(TARGET_ADDRESS)?;
            let tx = asset_hub::transactions()
                .balances()
                .transfer_keep_alive(dest.into(), TRANSFER_AMOUNT);

            // Define the USDT asset location in XCM format
            let asset_location = Location {
                parents: 0,
                interior: Junctions::X2([
                    Junction::PalletInstance(50),
                    Junction::GeneralIndex(USDT_ASSET_ID),
                ]),
            };

            // Build transaction params to pay fees with the alternative asset
            let tx_params = DefaultExtrinsicParamsBuilder::<AssetHubConfig>::new()
                .tip_of(0, asset_location)
                .build();

            // Sign, submit, and watch for finalization
            println!("Signing and submitting transaction...");
            let progress = at_block
                .transactions()
                .sign_and_submit_then_watch(&tx, &alice, tx_params)
                .await?;

            let in_block = progress.wait_for_finalized().await?;
            let block_hash = in_block.block_hash();
            let events = in_block.wait_for_success().await?;

            // Display transaction results
            println!("\nTransaction finalized in block: {:?}", block_hash);

            println!("\nEvents:");
            for event in events.iter().{}",
                    event.pallet_name(),
                    event.event_name()
                );
            }

            Ok(())
        }
        ```

### Run the Script

=== "PAPI"

    ```bash
    npx ts-node fee-payment-transaction.ts
    ```

=== "Polkadot.js"

    ```bash
    node fee-payment-transaction.js
    ```

=== "Subxt"

    ```bash
    cargo run --bin fee_payment_transaction
    ```

### Expected Output

When you run the script successfully, you should see output similar to:

=== "PAPI"

    <div id="termynal" data-termynal>
      <span data-ty="input"><span class="file-path"></span>npx ts-node fee-payment-transaction.ts</span>
      <pre>
    Tx finalized: 0xfe4e3fa64d374e256c72463c507743f16672caaf1b4e539fe913026de394009e (ok=true)
    Block: #12255461 0xaf315c306304ad175e4e24c5c8cbf97518c1411183bbf81a6107209a49d84f4d [tx index 2]
    Events:
    - Assets
    - Balances
    - Assets
    - AssetConversion
    - Balances
    - System
    - Balances
    - Balances
    - Balances
    - AssetTxPayment
    - System
    </pre>
    </div>
=== "Polkadot.js"

    <div id="termynal" data-termynal>
      <span data-ty="input"><span class="file-path"></span>node fee-payment-transaction.js</span>
      <pre>
    Connected to Polkadot Hub (Chopsticks fork)
    Signing and submitting transaction...

    Transaction finalized in block: 0x1f4849218bb4c04564a6c6f69c9e40a3940dcdabdc089da01bb49fb471a2c049
    Transaction hash: 0x9c967bb79fd09579f5e530a0446ce0171efe9241ba5957d6bcba80bccd5f66da

    Events:
      assets.Withdrawn
      balances.Withdraw
      assets.Deposited
      assetConversion.SwapCreditExecuted
      balances.Upgraded
      system.NewAccount
      balances.Endowed
      balances.Transfer
      balances.Deposit
      assetTxPayment.AssetTxFeePaid
      system.ExtrinsicSuccess
    </pre>
    </div>
=== "Subxt"

    <div id="termynal" data-termynal>
      <span data-ty="input"><span class="file-path"></span>cargo run --bin fee_payment_transaction</span>
      <pre>
    Connected to Polkadot Hub (Chopsticks fork)
    Sender (Alice): 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
    Signing and submitting transaction...

    Transaction finalized in block: 0x90c92e4dca64631ab4ccaabb14273cebbb3d59205db437dfcf9ace91452b1434

    Events:
      Assets.Withdrawn
      Balances.Withdraw
      Assets.Deposited
      AssetConversion.SwapCreditExecuted
      Balances.Upgraded
      System.NewAccount
      Balances.Endowed
      Balances.Transfer
      Balances.Deposit
      AssetTxPayment.AssetTxFeePaid
      System.ExtrinsicSuccess
    </pre>
    </div>
The key events to look for are:

- **`AssetTxPayment`**: Confirms the fees were paid using the alternative asset.
- **`AssetConversion`**: The alternative asset was swapped to cover native fees.
- **`Balances`**: The transfer was executed and deposit and withdrawal events were emitted.
- **`System`**: The transaction completed successfully.

## Conclusion

Paying transaction fees with alternative tokens on Polkadot Hub provides significant flexibility for users and applications.

The key takeaway is understanding how to specify alternative assets using the XCM location format, which opens up possibilities for building applications that can operate entirely using specific token ecosystems while still leveraging the full power of the network.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Send Transactions with SDKs__

    ---

    Learn how to send various types of transactions using different SDKs.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/with-sdks/)

-   <span class="badge guide">Guide</span> __Calculate Transaction Fees__

    ---

    Understand how to estimate transaction fees before submitting.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/calculate-transaction-fees/)

</div>
