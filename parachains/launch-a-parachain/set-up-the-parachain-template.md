---
title: Set Up the Polkadot SDK Parachain Template
description: Learn how to set up and run the Polkadot SDK Parachain Template locally, creating a ready-to-customize foundation for your parachain.
categories:
- Basics
- Parachains
url: https://docs.polkadot.com/parachains/launch-a-parachain/set-up-the-parachain-template/
word_count: 1627
token_estimate: 2599
version_hash: sha256:8fd837b2a15b663758011b2faafe8f1aa94dc6d514a1783637a10b28c15014be
last_updated: '2026-04-15T03:51:44+00:00'
---

# Set Up the Polkadot SDK Parachain Template

## Introduction

The [Polkadot SDK](https://github.com/paritytech/polkadot-sdk) includes several [templates](/polkadot-mkdocs/parachains/customize-runtime/#starting-templates) designed to help you quickly start building your own blockchain. Each template offers a different level of configuration, from minimal setups to feature-rich environments, allowing you to choose the foundation that best fits your project's needs.

Among these, the [Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template) provides a preconfigured runtime with commonly used pallets, making it an ideal starting point for most parachain development projects.

This guide walks you through the full process of working with this template. You will:

- Set up the Polkadot SDK Parachain Template.
- Understand the project structure and key components.
- Verify your template is ready for development.
- Run the parachain template locally in development mode.

By the end of this guide, you'll have a working template ready to customize and deploy as a parachain.

## Prerequisites

Before getting started, ensure you have done the following:

- Completed the [Install Polkadot SDK](/polkadot-mkdocs/parachains/install-polkadot-sdk/) guide and successfully installed [Rust](https://rust-lang.org/) and the required packages to set up your development environment.

For this tutorial series, you need to use Rust `1.88`. Newer versions of the compiler may not work with this parachain template version.

Run the following commands to set up the correct Rust version:

=== "macOS"

    ```bash
    rustup install 1.88
    rustup default 1.88
    rustup target add wasm32-unknown-unknown --toolchain 1.88-aarch64-apple-darwin
    rustup component add rust-src --toolchain 1.88-aarch64-apple-darwin
    ```

=== "Ubuntu"

    ```bash
    rustup toolchain install 1.88.0
    rustup default 1.88.0
    rustup target add wasm32-unknown-unknown --toolchain 1.88.0
    rustup component add rust-src --toolchain 1.88.0
    ```

## Polkadot SDK Utility Tools

This tutorial requires two essential tools:

- [**Chain spec builder**](https://crates.io/crates/staging-chain-spec-builder/16.0.0) - a Polkadot SDK utility for generating chain specifications. Refer to the [Generate Chain Specs](/polkadot-mkdocs/parachains/launch-a-parachain/deploy-to-polkadot/#generate-the-chain-specification) documentation for detailed usage
- [**Polkadot Omni Node**](https://crates.io/crates/polkadot-omni-node/0.13.2) - a white-labeled binary, released as a part of Polkadot SDK, that can act as the collator of a parachain in production, with all the related auxiliary functionalities that a normal collator node has: RPC server, archiving state, etc. It can also run the Wasm blob of the parachain locally for testing and development

Download the pre-built binaries from the [Polkadot SDK release](https://github.com/paritytech/polkadot-sdk/releases/tag/polkadot-stable2512-2) (recommended):

=== "macOS"

    ```bash
    curl -L -o chain-spec-builder https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/chain-spec-builder-aarch64-apple-darwin
    curl -L -o polkadot-omni-node https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/polkadot-omni-node-aarch64-apple-darwin
    chmod +x chain-spec-builder polkadot-omni-node
    sudo mv chain-spec-builder polkadot-omni-node /usr/local/bin/
    ```

=== "Ubuntu"

    ```bash
    curl -L -o chain-spec-builder https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/chain-spec-builder
    curl -L -o polkadot-omni-node https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/polkadot-omni-node
    chmod +x chain-spec-builder polkadot-omni-node
    sudo mv chain-spec-builder polkadot-omni-node /usr/local/bin/
    ```

Alternatively, you can install from source using `cargo`:

```bash
cargo install --locked staging-chain-spec-builder@16.0.0
cargo install --locked polkadot-omni-node@0.13.2
```

## Clone the Template

The [Polkadot SDK Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template) provides a ready-to-use development environment for building with the [Polkadot SDK](https://github.com/paritytech/polkadot-sdk). Follow these steps to set up the template:

1. Clone the template repository:

    ```bash
    git clone https://github.com/paritytech/polkadot-sdk-parachain-template.git parachain-template
    ```

2. Navigate into the project directory:

    ```bash
    cd parachain-template
    ```

## Explore the Project Structure

Before building the template, take a moment to familiarize yourself with its structure. Understanding this organization will help you navigate the codebase as you develop your parachain.

The template follows a standard Polkadot SDK project layout:

```text
parachain-template/
├── node/              # Node implementation and client
├── pallets/           # Custom pallets for your parachain
├── runtime/           # Runtime configuration and logic
├── Cargo.toml         # Workspace configuration
└── README.md          # Documentation
```

Key directories explained:

- **runtime/**: Contains your parachain's state transition function and pallet configuration. This is where you'll define what your blockchain can do.
- **node/**: Houses the client implementation that runs your blockchain, handles networking, and manages the database.
- **pallets/**: Where you'll create custom business logic modules (pallets) for your specific use case.
- **Cargo.toml**: The workspace configuration that ties all components together.

!!!note
    The runtime is compiled to WebAssembly (Wasm), enabling forkless upgrades. The node binary remains constant while the runtime can be updated on-chain.

## Compile the Runtime

Now that you understand the template structure, let's compile the runtime to ensure everything is working correctly.

1. Compile the runtime:

    ```bash
    cargo build --release --locked
    ```

    !!!tip
        Initial compilation may take several minutes, depending on your machine specifications. Use the `--release` flag for improved runtime performance compared to the default `--debug` build. If you need to troubleshoot issues, the `--debug` build provides better diagnostics.
        
        For production deployments, consider using a dedicated `--profile production` flag - this can provide an additional 15-30% performance improvement over the standard `--release` profile.

2. Upon successful compilation, you should see output indicating the build was successful. The compiled runtime will be located at:
    
    `./target/release/wbuild/parachain-template-runtime/parachain_template_runtime.compact.compressed.wasm`

## Verify the Build

After compilation completes, verify that the runtime was created successfully by checking for the Wasm blob:

```bash
ls -la ./target/release/wbuild/parachain-template-runtime/
```

You should see the `parachain_template_runtime.compact.compressed.wasm` file in the output, confirming the build was successful.

## Run the Node Locally

After successfully compiling your runtime, you can spin up a local chain and produce blocks. This process will start your local parachain using the Polkadot Omni Node and allow you to interact with it. You'll first need to generate a chain specification that defines your network's identity, initial connections, and genesis state, providing the foundational configuration for how your nodes connect and what initial state they agree upon.

Follow these steps to launch your node in development mode:

1. Generate the chain specification file of your parachain:

    ```bash
    chain-spec-builder create -t development \
    --relay-chain paseo \
    --para-id 1000 \
    --runtime ./target/release/wbuild/parachain-template-runtime/parachain_template_runtime.compact.compressed.wasm \
    named-preset development
    ```

2. Start the Omni Node with the generated chain spec. You'll start it in development mode (without a relay chain config), producing and finalizing blocks:

    ```bash
    polkadot-omni-node --chain ./chain_spec.json --dev
    ```

    The `--dev` option does the following:

    - Deletes all active data (keys, blockchain database, networking information) when stopped.
    - Ensures a clean working state each time you restart the node.

3. Verify that your node is running by reviewing the terminal output. You should see log messages indicating block production and finalization.

4. Confirm that your blockchain is producing new blocks by checking if the number after `finalized` is increasing in the output.

The details of the log output will be explored in a later tutorial. For now, knowing that your node is running and producing blocks is sufficient.

## Interact with the Node

When running the template node, it's accessible by default at `ws://localhost:9944`. To interact with your node using the [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) interface, follow these steps:

1. Open [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) in your web browser and click the network icon (which should be the Polkadot logo) in the top left corner:
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-01.webp)

2. Connect to your local node:

    1. Scroll to the bottom and select **Development**.
    2. Choose **Custom**.
    3. Enter `ws://localhost:9944` in the **custom endpoint** input field.
    4. Click the **Switch** button.
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-02.webp)

3. Once connected, you should see **parachain-template-runtime** in the top left corner, with the interface displaying information about your local blockchain.
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-03.webp)

You are now connected to your local node and can interact with it through the Polkadot.js Apps interface. This tool enables you to explore blocks, execute transactions, and interact with your blockchain's features. For in-depth guidance on using the interface effectively, refer to the [Polkadot.js Guides](https://wiki.polkadot.com/general/polkadotjs/) available on the Polkadot Wiki.

## Stop the Node

When you're done exploring your local node, you can stop it to remove any state changes you've made. Since you started the node with the `--dev` option, stopping the node will purge all persistent block data, allowing you to start fresh the next time.

To stop the local node:

1. Return to the terminal window where the node output is displayed.
2. Press `Control-C` to stop the running process.
3. Verify that your terminal returns to the prompt in the `parachain-template` directory.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge tutorial">Tutorial</span> __Deploy to Polkadot__

    ---

    Learn how to deploy your parachain template to a relay chain testnet. Configure your chain specification, register as a parachain, and start producing blocks.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/launch-a-parachain/deploy-to-polkadot/)

</div>
