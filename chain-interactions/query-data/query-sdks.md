---
title: Query On-Chain State with SDKs
description: Query on-chain storage (System.Account balances, Assets pallet metadata) on Polkadot Hub using PAPI (`polkadot-api`), Polkadot.js API (`@polkadot/api`, maintenance mode — prefer PAPI or Dedot for new projects), Dedot, Python Substrate Interface (`substrate-interface`), or Subxt (Rust). Covers both account balance and asset information queries with runnable code examples. Requires Node.js 18+, Python 3.8+, or Rust stable toolchain depending on SDK.
categories:
- Chain Interactions
- Tooling
url: https://docs.polkadot.com/chain-interactions/query-data/query-sdks/
word_count: 3760
token_estimate: 8001
version_hash: sha256:d52138ce820d0d15310cfbe2308ad904d1855f9465054078f8e84fc1f6cc6114
last_updated: '2026-04-15T03:51:44+00:00'
---

Covers: PAPI (polkadot-api), Polkadot.js API (@polkadot/api), Dedot, Python Substrate Interface (substrate-interface), Subxt, System.Account storage queries, Assets pallet metadata queries
Requires: Node.js 18+ with npm/pnpm/yarn (PAPI, Polkadot.js, Dedot), Python 3.8+ with pip (PSI), or Rust stable with Cargo (Subxt); WebSocket endpoint for Polkadot Hub
Returns: Native token balance (free, reserved, frozen) and asset metadata including name, symbol, decimals, and account asset balance

# Query On-Chain State with SDKs

## Query On-Chain Data

=== "PAPI"

    **Prerequisites**

    - [Node.js](https://nodejs.org/) v18 or higher
    - npm, pnpm, or yarn package manager

    **Environment Setup**

    1. Create and initialize a new project:

        ```bash
        mkdir papi-query-example && cd papi-query-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install polkadot-api && \
        npm install --save-dev @types/node tsx typescript
        ```

    3. Generate types for Polkadot Hub:

        ```bash
        npx papi add pah -n polkadot_asset_hub
        ```

    **Query Account Balance**

    The following example queries the `System.Account` storage to retrieve an account's native token balance.

    Create a file named `query-balance.ts` and add the following code to it:

    ```typescript title="query-balance.ts"
    import { createClient } from 'polkadot-api';
    import { getWsProvider } from 'polkadot-api/ws-provider/node';
    import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
    import { pah } from '@polkadot-api/descriptors';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query (Polkadot Hub address)
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Query the System.Account storage
      const accountInfo = await api.query.System.Account.getValue(ADDRESS);

      // Extract balance information
      const { data, nonce } = accountInfo;
      const { free, reserved, frozen } = data;

      console.log('Account Information:');
      console.log(`  Nonce: ${nonce}`);
      console.log(`  Free Balance: ${free}`);
      console.log(`  Reserved: ${reserved}`);
      console.log(`  Frozen: ${frozen}`);

      // Disconnect the client
      client.destroy();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx query-balance.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx query-balance.ts</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>Account Information:</span>
        <span data-ty>  Nonce: 0</span>
        <span data-ty>  Free Balance: 0</span>
        <span data-ty>  Reserved: 0</span>
        <span data-ty>  Frozen: 0</span>
    </div>
    **Query Asset Information**

    The following example queries the `Assets` pallet to retrieve metadata and balance information for USDT (asset ID 1984).

    Create a file named `query-asset.ts` and add the following code to it:

    ```typescript title="query-asset.ts"
    import { createClient } from 'polkadot-api';
    import { getWsProvider } from 'polkadot-api/ws-provider/node';
    import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
    import { pah } from '@polkadot-api/descriptors';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // USDT on Polkadot Hub
    const USDT_ASSET_ID = 1984;

    // Example address to query asset balance
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Query asset metadata
      const assetMetadata = await api.query.Assets.Metadata.getValue(USDT_ASSET_ID);

      if (assetMetadata)`);
        console.log(`  Symbol: ${assetMetadata.symbol.asText()}`);
        console.log(`  Decimals: ${assetMetadata.decimals}`);
      }

      // Query asset details
      const assetDetails = await api.query.Assets.Asset.getValue(USDT_ASSET_ID);

      if (assetDetails)`);
        console.log(`  Supply: ${assetDetails.supply}`);
        console.log(`  Accounts: ${assetDetails.accounts}`);
        console.log(`  Min Balance: ${assetDetails.min_balance}`);
        console.log(`  Status: ${assetDetails.status.type}`);
      }

      // Query account's asset balance
      console.log(`\nQuerying asset balance for: ${ADDRESS}`);
      const assetAccount = await api.query.Assets.Account.getValue(
        USDT_ASSET_ID,
        ADDRESS
      );

      if (assetAccount)`);
        console.log(`  Status: ${assetAccount.status.type}`);
      } else {
        console.log('\nNo asset balance found for this account');
      }

      // Disconnect the client
      client.destroy();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx query-asset.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx query-asset.ts</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying asset ID: 1984</span>
        <span data-ty></span>
        <span data-ty>Asset Metadata:</span>
        <span data-ty>  Name: Tether USD</span>
        <span data-ty>  Symbol: USDT</span>
        <span data-ty>  Decimals: 6</span>
        <span data-ty></span>
        <span data-ty>Asset Details:</span>
        <span data-ty>  Owner: 15uPcYeUE2XaMiMJuR6W7QGW2LsLdKXX7F3PxKG8gcizPh3X</span>
        <span data-ty>  Supply: 77998622834581</span>
        <span data-ty>  Accounts: 13544</span>
        <span data-ty>  Min Balance: 10000</span>
        <span data-ty>  Status: Live</span>
        <span data-ty></span>
        <span data-ty>Querying asset balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>No asset balance found for this account</span>
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
        mkdir pjs-query-example && cd pjs-query-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install @polkadot/api
        ```

    **Query Account Balance**

    The following example queries the `System.Account` storage to retrieve an account's native token balance.

    Create a file named `query-balance.js` and add the following code to it:

    ```javascript title="query-balance.js"
    import { ApiPromise, WsProvider } from '@polkadot/api';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query (Polkadot Hub address)
    const ADDRESS = 'INSERT_ADDRESS';

    async function main());

      console.log('Connected to Polkadot Hub');
      console.log(`Querying balance for: ${ADDRESS}\n`);

      // Query the system.account storage
      const accountInfo = await api.query.system.account(ADDRESS);

      // Extract balance information
      const { nonce, data } = accountInfo;
      const { free, reserved, frozen } = data;

      console.log('Account Information:');
      console.log(`  Nonce: ${nonce.toString()}`);
      console.log(`  Free Balance: ${free.toString()}`);
      console.log(`  Reserved: ${reserved.toString()}`);
      console.log(`  Frozen: ${frozen.toString()}`);

      // Disconnect from the node
      await api.disconnect();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    node query-balance.js
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>node query-balance.js</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>Account Information:</span>
        <span data-ty>  Nonce: 0</span>
        <span data-ty>  Free Balance: 0</span>
        <span data-ty>  Reserved: 0</span>
        <span data-ty>  Frozen: 0</span>
    </div>
    **Query Asset Information**

    The following example queries the `Assets` pallet to retrieve metadata and balance information for USDT (asset ID 1984).

    Create a file named `query-asset.js` and add the following code to it:

    ```javascript title="query-asset.js"
    import { ApiPromise, WsProvider } from '@polkadot/api';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // USDT on Polkadot Hub
    const USDT_ASSET_ID = 1984;

    // Example address to query asset balance
    const ADDRESS = 'INSERT_ADDRESS';

    async function main());

      console.log('Connected to Polkadot Hub');
      console.log(`Querying asset ID: ${USDT_ASSET_ID}\n`);

      // Query asset metadata
      const assetMetadata = await api.query.assets.metadata(USDT_ASSET_ID);

      console.log('Asset Metadata:');
      console.log(`  Name: ${assetMetadata.name.toUtf8()}`);
      console.log(`  Symbol: ${assetMetadata.symbol.toUtf8()}`);
      console.log(`  Decimals: ${assetMetadata.decimals.toString()}`);

      // Query asset details
      const assetDetails = await api.query.assets.asset(USDT_ASSET_ID);

      if (assetDetails.isSome)`);
        console.log(`  Supply: ${details.supply.toString()}`);
        console.log(`  Accounts: ${details.accounts.toString()}`);
        console.log(`  Min Balance: ${details.minBalance.toString()}`);
        console.log(`  Status: ${details.status.type}`);
      }

      // Query account's asset balance
      console.log(`\nQuerying asset balance for: ${ADDRESS}`);
      const assetAccount = await api.query.assets.account(USDT_ASSET_ID, ADDRESS);

      if (assetAccount.isSome)`);
        console.log(`  Status: ${account.status.type}`);
      } else {
        console.log('\nNo asset balance found for this account');
      }

      // Disconnect from the node
      await api.disconnect();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.
    
    Run the script:

    ```bash
    node query-asset.js
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>node query-asset.js</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying asset ID: 1984</span>
        <span data-ty></span>
        <span data-ty>Asset Metadata:</span>
        <span data-ty>  Name: Tether USD</span>
        <span data-ty>  Symbol: USDT</span>
        <span data-ty>  Decimals: 6</span>
        <span data-ty></span>
        <span data-ty>Asset Details:</span>
        <span data-ty>  Owner: 15uPcYeUE2XaMiMJuR6W7QGW2LsLdKXX7F3PxKG8gcizPh3X</span>
        <span data-ty>  Supply: 77998622834581</span>
        <span data-ty>  Accounts: 13544</span>
        <span data-ty>  Min Balance: 10000</span>
        <span data-ty>  Status: Live</span>
        <span data-ty></span>
        <span data-ty>Querying asset balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>No asset balance found for this account</span>
    </div>
=== "Dedot"

    **Prerequisites**

    - [Node.js](https://nodejs.org/) v18 or higher
    - npm, pnpm, or yarn package manager

    **Environment Setup**

    1. Create and initialize a new project:

        ```bash
        mkdir dedot-query-example && cd dedot-query-example && \
        npm init -y && npm pkg set type=module
        ```

    2. Install dependencies:

        ```bash
        npm install dedot && \
        npm install --save-dev @dedot/chaintypes @types/node tsx typescript
        ```

    **Query Account Balance**

    The following example queries the `System.Account` storage to retrieve an account's native token balance.

    Create a file named `query-balance.ts` and add the following code to it:

    ```typescript title="query-balance.ts"
    import { DedotClient, WsProvider } from 'dedot';
    import type { PolkadotAssetHubApi } from '@dedot/chaintypes';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // Example address to query (Polkadot Hub address)
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Query the system.account storage
      const accountInfo = await client.query.system.account(ADDRESS);

      // Extract balance information
      const { nonce, data } = accountInfo;
      const { free, reserved, frozen } = data;

      console.log('Account Information:');
      console.log(`  Nonce: ${nonce}`);
      console.log(`  Free Balance: ${free}`);
      console.log(`  Reserved: ${reserved}`);
      console.log(`  Frozen: ${frozen}`);

      // Disconnect the client
      await client.disconnect();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx query-balance.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx query-balance.ts</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>Account Information:</span>
        <span data-ty>  Nonce: 0</span>
        <span data-ty>  Free Balance: 0</span>
        <span data-ty>  Reserved: 0</span>
        <span data-ty>  Frozen: 0</span>
    </div>
    **Query Asset Information**

    The following example queries the `Assets` pallet to retrieve metadata and balance information for USDT (asset ID 1984).

    Create a file named `query-asset.ts` and add the following code to it:

    ```typescript title="query-asset.ts"
    import { DedotClient, WsProvider } from 'dedot';
    import { hexToString } from 'dedot/utils';
    import type { PolkadotAssetHubApi } from '@dedot/chaintypes';

    const ASSET_HUB_RPC = 'INSERT_WS_ENDPOINT';

    // USDT on Polkadot Hub
    const USDT_ASSET_ID = 1984;

    // Example address to query asset balance
    const ADDRESS = 'INSERT_ADDRESS';

    async function main()\n`);

      // Query asset metadata
      const assetMetadata = await client.query.assets.metadata(USDT_ASSET_ID);

      console.log('Asset Metadata:');
      console.log(`  Name: ${hexToString(assetMetadata.name)}`);
      console.log(`  Symbol: ${hexToString(assetMetadata.symbol)}`);
      console.log(`  Decimals: ${assetMetadata.decimals}`);

      // Query asset details
      const assetDetails = await client.query.assets.asset(USDT_ASSET_ID);

      if (assetDetails)`);
        console.log(`  Supply: ${assetDetails.supply}`);
        console.log(`  Accounts: ${assetDetails.accounts}`);
        console.log(`  Min Balance: ${assetDetails.minBalance}`);
        console.log(`  Status: ${JSON.stringify(assetDetails.status)}`);
      }

      // Query account's asset balance
      console.log(`\nQuerying asset balance for: ${ADDRESS}`);
      const assetAccount = await client.query.assets.account([
        USDT_ASSET_ID,
        ADDRESS,
      ]);

      if (assetAccount)`);
        console.log(`  Status: ${JSON.stringify(assetAccount.status)}`);
      } else {
        console.log('\nNo asset balance found for this account');
      }

      // Disconnect the client
      await client.disconnect();
    }

    main().catch(console.error);
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    npx tsx query-asset.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx query-asset.ts</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying asset ID: 1984</span>
        <span data-ty></span>
        <span data-ty>Asset Metadata:</span>
        <span data-ty>  Name: Tether USD</span>
        <span data-ty>  Symbol: USDT</span>
        <span data-ty>  Decimals: 6</span>
        <span data-ty></span>
        <span data-ty>Asset Details:</span>
        <span data-ty>  Owner: 5Gy6UDPQNFG6vBLnwn3VyFSMAisgw1yP2kJuo2Gn8XhUD8V8</span>
        <span data-ty>  Supply: 77998622834581</span>
        <span data-ty>  Accounts: 13544</span>
        <span data-ty>  Min Balance: 10000</span>
        <span data-ty>  Status: "Live"</span>
        <span data-ty></span>
        <span data-ty>Querying asset balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>No asset balance found for this account</span>
    </div>
=== "Py Substrate Interface"

    **Prerequisites**

    - [Python](https://www.python.org/) 3.8 or higher
    - pip package manager

    **Environment Setup**

    1. Create a new project directory and set up a virtual environment:

        ```bash
        mkdir psi-query-example && cd psi-query-example && \
        python3 -m venv venv && source venv/bin/activate
        ```

    2. Install the substrate-interface package:

        ```bash
        pip install substrate-interface
        ```

    **Query Account Balance**

    The following example queries the `System.Account` storage to retrieve an account's native token balance.

    Create a file named `query_balance.py` and add the following code to it:

    ```python title="query_balance.py"
    from substrateinterface import SubstrateInterface

    ASSET_HUB_RPC = "INSERT_WS_ENDPOINT"

    # Example address to query (Polkadot Hub address)
    ADDRESS = "INSERT_ADDRESS"


    def main():
        # Connect to Polkadot Hub
        substrate = SubstrateInterface(url=ASSET_HUB_RPC)

        print("Connected to Polkadot Hub")
        print(f"Querying balance for: {ADDRESS}\n")

        # Query the System.Account storage
        account_info = substrate.query(
            module="System",
            storage_function="Account",
            params=[ADDRESS],
        )

        # Extract balance information
        nonce = account_info.value["nonce"]
        data = account_info.value["data"]
        free = data["free"]
        reserved = data["reserved"]
        frozen = data["frozen"]

        print("Account Information:")
        print(f"  Nonce: {nonce}")
        print(f"  Free Balance: {free}")
        print(f"  Reserved: {reserved}")
        print(f"  Frozen: {frozen}")

        # Close connection
        substrate.close()


    if __name__ == "__main__":
        main()
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    python query_balance.py
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>python query_balance.py</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>Account Information:</span>
        <span data-ty>  Nonce: 0</span>
        <span data-ty>  Free Balance: 0</span>
        <span data-ty>  Reserved: 0</span>
        <span data-ty>  Frozen: 0</span>
    </div>
    **Query Asset Information**

    The following example queries the `Assets` pallet to retrieve metadata and balance information for USDT (asset ID 1984).

    Create a file named `query_asset.py` and add the following code to it:

    ```python title="query_asset.py"
    from substrateinterface import SubstrateInterface

    ASSET_HUB_RPC = "INSERT_WS_ENDPOINT"

    # USDT on Polkadot Hub
    USDT_ASSET_ID = 1984

    # Example address to query asset balance
    ADDRESS = "INSERT_ADDRESS"


    def main():
        # Connect to Polkadot Hub
        substrate = SubstrateInterface(url=ASSET_HUB_RPC)

        print("Connected to Polkadot Hub")
        print(f"Querying asset ID: {USDT_ASSET_ID}\n")

        # Query asset metadata
        asset_metadata = substrate.query(
            module="Assets",
            storage_function="Metadata",
            params=[USDT_ASSET_ID],
        )

        if asset_metadata.value:
            metadata = asset_metadata.value
            print("Asset Metadata:")
            print(f"  Name: {metadata['name']}")
            print(f"  Symbol: {metadata['symbol']}")
            print(f"  Decimals: {metadata['decimals']}")

        # Query asset details
        asset_details = substrate.query(
            module="Assets",
            storage_function="Asset",
            params=[USDT_ASSET_ID],
        )

        if asset_details.value:
            details = asset_details.value
            print("\nAsset Details:")
            print(f"  Owner: {details['owner']}")
            print(f"  Supply: {details['supply']}")
            print(f"  Accounts: {details['accounts']}")
            print(f"  Min Balance: {details['min_balance']}")
            print(f"  Status: {details['status']}")

        # Query account's asset balance
        print(f"\nQuerying asset balance for: {ADDRESS}")
        asset_account = substrate.query(
            module="Assets",
            storage_function="Account",
            params=[USDT_ASSET_ID, ADDRESS],
        )

        if asset_account.value:
            account = asset_account.value
            print("\nAsset Account:")
            print(f"  Balance: {account['balance']}")
            print(f"  Status: {account['status']}")
        else:
            print("\nNo asset balance found for this account")

        # Close connection
        substrate.close()


    if __name__ == "__main__":
        main()
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    python query_asset.py
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>python query_asset.py</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying asset ID: 1984</span>
        <span data-ty></span>
        <span data-ty>Asset Metadata:</span>
        <span data-ty>  Name: Tether USD</span>
        <span data-ty>  Symbol: USDT</span>
        <span data-ty>  Decimals: 6</span>
        <span data-ty></span>
        <span data-ty>Asset Details:</span>
        <span data-ty>  Owner: 5Gy6UDPQNFG6vBLnwn3VyFSMAisgw1yP2kJuo2Gn8XhUD8V8</span>
        <span data-ty>  Supply: 77998622834581</span>
        <span data-ty>  Accounts: 13545</span>
        <span data-ty>  Min Balance: 10000</span>
        <span data-ty>  Status: Live</span>
        <span data-ty></span>
        <span data-ty>Querying asset balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>No asset balance found for this account</span>
    </div>
=== "Subxt"

    [Subxt](/polkadot-mkdocs/reference/tools/subxt/) is a Rust library that provides compile-time type safety through code generation from chain metadata.

    **Prerequisites**

    - [Rust](https://rustup.rs/) toolchain (latest stable)
    - Cargo package manager

    **Environment Setup**

    1. Create a new Rust project:

        ```bash
        cargo new subxt-query-example && cd subxt-query-example
        ```

    2. Install the Subxt CLI:

        ```bash
        cargo install subxt-cli@0.50.0
        ```

    3. Download the Polkadot Hub metadata:

        ```bash
        subxt metadata --url INSERT_WS_ENDPOINT -o asset_hub_metadata.scale
        ```

        !!! note
            Ensure to replace `INSERT_WS_ENDPOINT` with the proper WebSocket endpoint, such as `wss://polkadot-asset-hub-rpc.polkadot.io` for Polkadot Hub.

    4. Update `Cargo.toml` with the required dependencies:

        ```toml title="Cargo.toml"
        [package]
        name = "subxt-query-example"
        version = "0.1.0"
        edition = "2021"

        [[bin]]
        name = "query_balance"
        path = "src/bin/query_balance.rs"

        [[bin]]
        name = "query_asset"
        path = "src/bin/query_asset.rs"

        [dependencies]
        subxt = "0.50.0"
        tokio = { version = "1", features = ["rt", "macros"] }
        ```

    **Query Account Balance**

    The following example queries the `System.Account` storage to retrieve an account's native token balance.

    Create a file at `src/bin/query_balance.rs` and add the following code to it:

    ```rust title="src/bin/query_balance.rs"
    use std::str::FromStr;
    use subxt::utils::AccountId32;
    use subxt::{OnlineClient, PolkadotConfig};

    // Generate an interface from the node's metadata
    #[subxt::subxt(runtime_metadata_path = "asset_hub_metadata.scale")]
    pub mod asset_hub {}

    const ASSET_HUB_RPC: &str = "INSERT_WS_ENDPOINT";

    // Example address to query (Polkadot Hub address)
    const ADDRESS: &str = "INSERT_ADDRESS";

    #[tokio::main(flavor = "current_thread")]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
        // Initialize the Subxt client
        let api = OnlineClient::<PolkadotConfig>::from_url(ASSET_HUB_RPC).await?;

        // Anchor to the current block
        let at_block = api.at_current_block().await?;

        println!("Connected to Polkadot Hub");
        println!("Querying balance for: {}\n", ADDRESS);

        // Parse the address
        let account = AccountId32::from_str(ADDRESS)?;

        // Build storage entry for System.Account
        let account_storage = at_block
            .storage()
            .entry(asset_hub::storage().system().account())?;

        // Fetch the account information
        let info = account_storage
            .fetch((account,))
            .await?
            .decode()?;

        println!("Account Information:");
        println!("  Nonce: {}", info.nonce);
        println!("  Free Balance: {}", info.data.free);
        println!("  Reserved: {}", info.data.reserved);
        println!("  Frozen: {}", info.data.frozen);

        Ok(())
    }
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    cargo run --bin query_balance
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>cargo run --bin query_balance</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>Account not found</span>
    </div>
    !!! note
        Subxt's `fetch()` method returns `None` for accounts with zero balance that have no on-chain storage entry, resulting in "Account not found". Accounts with activity will display their balance information.

    **Query Asset Information**

    The following example queries the `Assets` pallet to retrieve metadata and balance information for USDT (asset ID 1984).

    Create a file at `src/bin/query_asset.rs` and add the following code to it:

    ```rust title="src/bin/query_asset.rs"
    use std::str::FromStr;
    use subxt::utils::AccountId32;
    use subxt::{OnlineClient, PolkadotConfig};

    // Generate an interface from the node's metadata
    #[subxt::subxt(runtime_metadata_path = "asset_hub_metadata.scale")]
    pub mod asset_hub {}

    const ASSET_HUB_RPC: &str = "INSERT_WS_ENDPOINT";

    // USDT on Polkadot Hub
    const USDT_ASSET_ID: u32 = 1984;

    // Example address to query asset balance
    const ADDRESS: &str = "INSERT_ADDRESS";

    #[tokio::main(flavor = "current_thread")]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
        // Initialize the Subxt client
        let api = OnlineClient::<PolkadotConfig>::from_url(ASSET_HUB_RPC).await?;

        // Anchor to the current block
        let at_block = api.at_current_block().await?;

        println!("Connected to Polkadot Hub");
        println!("Querying asset ID: {}\n", USDT_ASSET_ID);

        // Query asset metadata
        let metadata_storage = at_block
            .storage()
            .entry(asset_hub::storage().assets().metadata())?;
        let meta = metadata_storage
            .fetch((USDT_ASSET_ID,))
            .await?
            .decode()?;

        println!("Asset Metadata:");
        println!(
            "  Name: {}",
            String::from_utf8_lossy(&meta.name.0)
        );
        println!(
            "  Symbol: {}",
            String::from_utf8_lossy(&meta.symbol.0)
        );
        println!("  Decimals: {}", meta.decimals);

        // Query asset details
        let asset_storage = at_block
            .storage()
            .entry(asset_hub::storage().assets().asset())?;
        let details = asset_storage
            .fetch((USDT_ASSET_ID,))
            .await?
            .decode()?;

        println!("\nAsset Details:");
        println!("  Owner: {}", details.owner);
        println!("  Supply: {}", details.supply);
        println!("  Accounts: {}", details.accounts);
        println!("  Min Balance: {}", details.min_balance);

        // Query account's asset balance
        let account = AccountId32::from_str(ADDRESS)?;
        println!("\nQuerying asset balance for: {}", ADDRESS);

        let account_storage = at_block
            .storage()
            .entry(asset_hub::storage().assets().account())?;
        let asset_account = account_storage
            .fetch((USDT_ASSET_ID, account))
            .await?
            .decode()?;

        println!("\nAsset Account:");
        println!("  Balance: {}", asset_account.balance);

        Ok(())
    }
    ```

    !!! note
        Ensure to replace `INSERT_WS_ENDPOINT` with a valid WebSocket endpoint (e.g., `wss://polkadot-asset-hub-rpc.polkadot.io`) and `INSERT_ADDRESS` with the account address you want to query.

    Run the script:

    ```bash
    cargo run --bin query_asset
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>cargo run --bin query_asset</span>
        <span data-ty>Connected to Polkadot Hub</span>
        <span data-ty>Querying asset ID: 1984</span>
        <span data-ty></span>
        <span data-ty>Asset Metadata:</span>
        <span data-ty>  Name: Tether USD</span>
        <span data-ty>  Symbol: USDT</span>
        <span data-ty>  Decimals: 6</span>
        <span data-ty></span>
        <span data-ty>Asset Details:</span>
        <span data-ty>  Owner: 5Gy6UDPQNFG6vBLnwn3VyFSMAisgw1yP2kJuo2Gn8XhUD8V8</span>
        <span data-ty>  Supply: 77998622834581</span>
        <span data-ty>  Accounts: 13545</span>
        <span data-ty>  Min Balance: 10000</span>
        <span data-ty></span>
        <span data-ty>Querying asset balance for: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3</span>
        <span data-ty></span>
        <span data-ty>No asset balance found for this account</span>
    </div>
