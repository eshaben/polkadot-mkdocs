---
title: Runtime API Calls
description: Execute Polkadot SDK runtime APIs — including AccountNonceApi, Metadata, TransactionPaymentApi, and StakingApi — against Polkadot Hub TestNet using PAPI (`polkadot-api`), Polkadot.js API (`@polkadot/api`, maintenance mode), Dedot, Python Substrate Interface, or Subxt (Rust). Unlike storage queries, runtime APIs invoke Wasm runtime logic to return computed results. Requires Node.js 18+, Python 3.8+, or Rust stable toolchain depending on chosen SDK.
categories:
- Polkadot Protocol
- Tooling
url: https://docs.polkadot.com/chain-interactions/query-data/runtime-api-calls/
word_count: 2077
token_estimate: 4247
version_hash: sha256:31b77160c8e65d6cbea0832192d8b3eedc754f589e82d0316a42002057c0d02a
last_updated: '2026-04-15T03:51:44+00:00'
---

Covers: Polkadot runtime APIs, PAPI, Polkadot.js API, Dedot, Python Substrate Interface, Subxt, AccountNonceApi, Metadata API, WebAssembly runtime execution
Requires: Node.js 18+ with npm/pnpm/yarn (PAPI, Polkadot.js, Dedot), Python 3.8+ with pip (PSI), or Rust stable with Cargo (Subxt); WebSocket endpoint for Polkadot Hub TestNet
Returns: Account transaction nonce, supported metadata versions

# Runtime API Calls

## Call Runtime APIs

This section demonstrates how to call runtime APIs using the following SDKs:

- **Polkadot API (PAPI)**: Modern TypeScript library with type-safe APIs.
- **Polkadot.js API**: Comprehensive JavaScript library (maintenance mode).
- **Dedot**: Lightweight TypeScript library optimized for performance.
- **Python Substrate Interface**: Python library for Substrate chains.
- **Subxt**: Rust library with compile-time type safety.

Select your preferred SDK below to see complete, runnable examples that query Polkadot Hub TestNet for account nonces and metadata information.

=== "PAPI"

    **Prerequisites**

    - [Node.js](https://nodejs.org/) v18 or higher
    - npm, pnpm, or yarn package manager

    **Environment Setup**

    1. Create and initialize a new project:

        ```bash
        mkdir papi-runtime-api-example && cd papi-runtime-api-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install polkadot-api && \
        npm install --save-dev @types/node tsx typescript
        ```

    3. Generate types for Polkadot Hub TestNet:

        ```bash
        npx papi add polkadotTestNet -w wss://asset-hub-paseo.dotters.network
        ```

    **Call Runtime APIs**

    The following example demonstrates calling several runtime APIs:

    - **`AccountNonceApi.account_nonce`**: Gets the current nonce for an account.
    - **`Metadata.metadata_versions`**: Retrieves supported metadata versions.

    Create a file named `runtime-apis.ts` and add the following code:

    ```typescript title="runtime-apis.ts"
    import { createClient } from 'polkadot-api';
    import { getWsProvider } from 'polkadot-api/ws-provider/node';
    import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
    import { polkadotTestNet } from '@polkadot-api/descriptors';

    const POLKADOT_TESTNET_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Call AccountNonceApi to get the account nonce
      const nonce = await api.apis.AccountNonceApi.account_nonce(ADDRESS);
      console.log('AccountNonceApi Results:');
      console.log(`  Account Nonce: ${nonce}`);

      // Query metadata versions using Metadata runtime API
      const metadataVersions = await api.apis.Metadata.metadata_versions();
      console.log('\nMetadata API Results:');
      console.log(`  Supported Metadata Versions: [${metadataVersions.join(', ')}]`);

      // Disconnect the client
      client.destroy();
    }

    main().catch(console.error);
    ```

    Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://asset-hub-paseo.dotters.network`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx runtime-apis.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx runtime-apis.ts</span>
        <span data-ty="progress"></span>
        <span data-ty>Connected to Polkadot Hub TestNet</span>
        <span data-ty>Querying runtime APIs for: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty></span>
        <span data-ty>AccountNonceApi Results:</span>
        <span data-ty>  Account Nonce: 11</span>
        <span data-ty></span>
        <span data-ty>Metadata API Results:</span>
        <span data-ty>  Supported Metadata Versions: [14, 15, 16]</span>
    </div>
=== "Polkadot.js"

    !!! warning "Maintenance Mode Only"
        The Polkadot.js API is no longer actively developed. New projects should use [PAPI](/polkadot-mkdocs/reference/tools/papi/) or [Dedot](/polkadot-mkdocs/reference/tools/dedot/) as actively maintained alternatives.

    **Prerequisites**

    - [Node.js](https://nodejs.org/) v18 or higher
    - npm, pnpm, or yarn package manager

    **Environment Setup**

    1. Create and initialize a new project:

        ```bash
        mkdir pjs-runtime-api-example && cd pjs-runtime-api-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install @polkadot/api
        ```

    **Call Runtime APIs**

    The following example demonstrates calling several runtime APIs:

    - **`accountNonceApi.accountNonce`**: Gets the current nonce for an account.
    - **`metadata.metadataVersions`**: Retrieves supported metadata versions.

    Create a file named `runtime-apis.js` and add the following code:

    ```javascript title="runtime-apis.js"
    import { ApiPromise, WsProvider } from '@polkadot/api';

    const POLKADOT_TESTNET_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query
    const ADDRESS = 'INSERT_ADDRESS';

    async function main());

      console.log('Connected to Polkadot Hub TestNet');
      console.log(`Querying runtime APIs for: ${ADDRESS}\n`);

      // Call AccountNonceApi to get the account nonce
      const nonce = await api.call.accountNonceApi.accountNonce(ADDRESS);
      console.log('AccountNonceApi Results:');
      console.log(`  Account Nonce: ${nonce.toString()}`);

      // Query metadata versions using Metadata runtime API
      const metadataVersions = await api.call.metadata.metadataVersions();
      console.log('\nMetadata API Results:');
      console.log(
        `  Supported Metadata Versions: [${metadataVersions.map((v) => v.toString()).join(', ')}]`
      );

      // Disconnect from the node
      await api.disconnect();
    }

    main().catch(console.error);
    ```

    Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://asset-hub-paseo.dotters.network`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    node runtime-apis.js
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>node runtime-apis.js</span>
        <span data-ty="progress"></span>
        <span data-ty>Connected to Polkadot Hub TestNet</span>
        <span data-ty>Querying runtime APIs for: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty></span>
        <span data-ty>AccountNonceApi Results:</span>
        <span data-ty>  Account Nonce: 11</span>
        <span data-ty></span>
        <span data-ty>Metadata API Results:</span>
        <span data-ty>  Supported Metadata Versions: [14, 15, 16]</span>
    </div>
=== "Dedot"

    **Prerequisites**

    - [Node.js](https://nodejs.org/) v18 or higher
    - npm, pnpm, or yarn package manager

    **Environment Setup**

    1. Create and initialize a new project:

        ```bash
        mkdir dedot-runtime-api-example && cd dedot-runtime-api-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install dedot && \
        npm install --save-dev @dedot/chaintypes @types/node tsx typescript
        ```

    **Call Runtime APIs**

    The following example demonstrates calling several runtime APIs:

    - **`accountNonceApi.accountNonce`**: Gets the current nonce for an account.
    - **`metadata.metadataVersions`**: Retrieves supported metadata versions.

    Create a file named `runtime-apis.ts` and add the following code:

    ```typescript title="runtime-apis.ts"
    import { DedotClient, WsProvider } from 'dedot';
    import type { PolkadotAssetHubApi } from '@dedot/chaintypes';

    const POLKADOT_TESTNET_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Call AccountNonceApi to get the account nonce
      const nonce = await client.call.accountNonceApi.accountNonce(ADDRESS);
      console.log('AccountNonceApi Results:');
      console.log(`  Account Nonce: ${nonce}`);

      // Query metadata versions using Metadata runtime API
      const metadataVersions = await client.call.metadata.metadataVersions();
      console.log('\nMetadata API Results:');
      console.log(`  Supported Metadata Versions: [${metadataVersions.join(', ')}]`);

      // Disconnect the client
      await client.disconnect();
    }

    main().catch(console.error);
    ```

    Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://asset-hub-paseo.dotters.network`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx runtime-apis.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx runtime-apis.ts</span>
        <span data-ty="progress"></span>
        <span data-ty>Connected to Polkadot Hub TestNet</span>
        <span data-ty>Querying runtime APIs for: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty></span>
        <span data-ty>AccountNonceApi Results:</span>
        <span data-ty>  Account Nonce: 11</span>
        <span data-ty></span>
        <span data-ty>Metadata API Results:</span>
        <span data-ty>  Supported Metadata Versions: [14, 15, 16]</span>
    </div>
=== "Py Substrate Interface"

    **Prerequisites**

    - [Python](https://www.python.org/) 3.8 or higher
    - pip package manager

    **Environment Setup**

    1. Create a new project directory and set up a virtual environment:

        ```bash
        mkdir psi-runtime-api-example && cd psi-runtime-api-example && \
        python3 -m venv venv && source venv/bin/activate
        ```

    2. Install the substrate-interface package:

        ```bash
        pip install substrate-interface
        ```

    **Call Runtime APIs**

    The following example demonstrates calling several runtime APIs:

    - **`AccountNonceApi.account_nonce`**: Gets the current nonce for an account.
    - **`Core.version`**: Retrieves runtime version information.

    Create a file named `runtime_apis.py` and add the following code:

    ```python title="runtime_apis.py"
    from substrateinterface import SubstrateInterface

    POLKADOT_TESTNET_RPC = "INSERT_WS_ENDPOINT"

    # Example address to query
    ADDRESS = "INSERT_ADDRESS"


    def main():
        # Connect to Polkadot Hub TestNet
        substrate = SubstrateInterface(url=POLKADOT_TESTNET_RPC)

        print("Connected to Polkadot Hub TestNet")
        print(f"Querying runtime APIs for: {ADDRESS}\n")

        # Call AccountNonceApi to get the account nonce
        nonce = substrate.runtime_call("AccountNonceApi", "account_nonce", [ADDRESS])
        print("AccountNonceApi Results:")
        print(f"  Account Nonce: {nonce.value}")

        # Query runtime version using Core runtime API
        version = substrate.runtime_call("Core", "version", [])
        print("\nCore API Results:")
        print(f"  Spec Name: {version.value['spec_name']}")
        print(f"  Spec Version: {version.value['spec_version']}")
        print(f"  Impl Version: {version.value['impl_version']}")

        # Close connection
        substrate.close()


    if __name__ == "__main__":
        main()
    ```

    Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://asset-hub-paseo.dotters.network`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    python runtime_apis.py
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>python runtime_apis.py</span>
        <span data-ty="progress"></span>
        <span data-ty>Connected to Polkadot Hub TestNet</span>
        <span data-ty>Querying runtime APIs for: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty></span>
        <span data-ty>AccountNonceApi Results:</span>
        <span data-ty>  Account Nonce: 11</span>
        <span data-ty></span>
        <span data-ty>Core API Results:</span>
        <span data-ty>  Spec Name: asset-hub-paseo</span>
        <span data-ty>  Spec Version: 1004001</span>
        <span data-ty>  Impl Version: 0</span>
    </div>
=== "Subxt"

    [Subxt](/polkadot-mkdocs/reference/tools/subxt/) is a Rust library that provides compile-time type safety through code generation from chain metadata.

    **Prerequisites**

    - [Rust](https://rustup.rs/) toolchain (latest stable)
    - Cargo package manager

    **Environment Setup**

    1. Create a new Rust project:

        ```bash
        cargo new subxt-runtime-api-example && cd subxt-runtime-api-example
        ```

    2. Install the Subxt CLI:

        ```bash
        cargo install subxt-cli@0.50.0
        ```

    3. Download the Polkadot Hub TestNet metadata:

        ```bash
        subxt metadata --url INSERT_WS_ENDPOINT -o polkadot_testnet_metadata.scale
        ```

        Ensure to replace `INSERT_WS_ENDPOINT` with the proper WebSocket endpoint, such as `wss://asset-hub-paseo.dotters.network` for Polkadot Hub TestNet.

    4. Update `Cargo.toml` with the required dependencies:

        ```toml title="Cargo.toml"
        [package]
        name = "subxt-runtime-api-example"
        version = "0.1.0"
        edition = "2021"

        [[bin]]
        name = "runtime_apis"
        path = "src/bin/runtime_apis.rs"

        [dependencies]
        subxt = "0.50.0"
        tokio = { version = "1", features = ["rt", "macros"] }
        ```

    **Call Runtime APIs**

    The following example demonstrates calling several runtime APIs:

    - **`AccountNonceApi.account_nonce`**: Gets the current nonce using the static interface.
    - **`Metadata.metadata_versions`**: Retrieves supported metadata versions using the dynamic interface.

    Create a file at `src/bin/runtime_apis.rs` and add the following code:

    ```rust title="src/bin/runtime_apis.rs"
    use std::str::FromStr;
    use subxt::dynamic::Value;
    use subxt::utils::AccountId32;
    use subxt::{OnlineClient, PolkadotConfig};

    // Generate an interface from the node's metadata
    #[subxt::subxt(runtime_metadata_path = "polkadot_testnet_metadata.scale")]
    pub mod polkadot_testnet {}

    const POLKADOT_TESTNET_RPC: &str = "INSERT_WS_ENDPOINT";

    // Example address to query
    const ADDRESS: &str = "INSERT_ADDRESS";

    #[tokio::main(flavor = "current_thread")]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
        // Initialize the Subxt client
        let api = OnlineClient::<PolkadotConfig>::from_url(POLKADOT_TESTNET_RPC).await?;

        // Anchor to the current block
        let at_block = api.at_current_block().await?;

        println!("Connected to Polkadot Hub TestNet");
        println!("Querying runtime APIs for: {}\n", ADDRESS);

        // Parse the address
        let account = AccountId32::from_str(ADDRESS)?;

        // Call AccountNonceApi using static interface
        let nonce_call = polkadot_testnet::runtime_apis()
            .account_nonce_api()
            .account_nonce(account.clone());
        let nonce = at_block.runtime_apis().call(nonce_call).await?;
        println!("AccountNonceApi Results:");
        println!("  Account Nonce: {}", nonce);

        // Call Metadata API to get supported versions using dynamic call
        let metadata_versions_call =
            subxt::dynamic::runtime_api_call::<_, Value>("Metadata", "metadata_versions", ());
        let versions_result = at_block
            .runtime_apis()
            .call(metadata_versions_call)
            .await?;
        println!("\nMetadata API Results:");
        println!(
            "  Supported Metadata Versions: {}",
            versions_result
        );

        Ok(())
    }
    ```

    Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://asset-hub-paseo.dotters.network`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    cargo run --bin runtime_apis
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>cargo run --bin runtime_apis</span>
        <span data-ty="progress"></span>
        <span data-ty>Connected to Polkadot Hub TestNet</span>
        <span data-ty>Querying runtime APIs for: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty></span>
        <span data-ty>AccountNonceApi Results:</span>
        <span data-ty>  Account Nonce: 11</span>
        <span data-ty></span>
        <span data-ty>Metadata API Results:</span>
        <span data-ty>  Supported Metadata Versions: (14, 15, 16)</span>
    </div>
## Available Runtime APIs

The following runtime APIs are commonly available on Polkadot SDK-based chains:

| API | Description |
|-----|-------------|
| `AccountNonceApi` | Get current transaction nonce for an account |
| `TransactionPaymentApi` | Query transaction fees and weight information |
| `TransactionPaymentCallApi` | Estimate fees for a call without creating an extrinsic |
| `Metadata` | Query metadata versions and runtime metadata |
| `BlockBuilder` | Access block building functionality |
| `Core` | Core runtime version and execution |
| `TaggedTransactionQueue` | Validate transactions in the pool |
| `OffchainWorkerApi` | Offchain worker functionality |
| `SessionKeys` | Session key management |
| `GrandpaApi` | GRANDPA finality information |
| `BabeApi` | BABE consensus information |
| `NominationPoolsApi` | Nomination pools data and pending rewards |
| `StakingApi` | Staking-related computations |

!!! note
    Available runtime APIs vary by chain. Check your target chain's metadata to see which APIs are exposed.
