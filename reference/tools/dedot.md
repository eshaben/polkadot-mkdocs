---
title: Dedot
description: Dedot is a next-gen JavaScript client for Polkadot and Polkadot SDK-based blockchains, offering lightweight, tree-shakable APIs with strong TypeScript support.
categories:
- Tooling
url: https://docs.polkadot.com/reference/tools/dedot/
word_count: 1020
token_estimate: 2113
version_hash: sha256:190fff8ee5ae724af1c7a323c3930fd0a0161d5b4d0fceea152e6e71ae73b269
last_updated: '2026-04-15T03:51:44+00:00'
---

# Dedot

## Introduction

[Dedot](https://github.com/dedotdev/dedot) is a next-generation JavaScript client for Polkadot and Polkadot SDK-based blockchains. Designed to elevate the dApp development experience, Dedot is built and optimized to be lightweight and tree-shakable, offering precise types and APIs suggestions for individual Polkadot SDK-based blockchains.

### Key Features

- **Lightweight and tree-shakable**: No more bn.js or WebAssembly blobs, optimized for dapps bundle size.
- **Fully typed API**: Comprehensive TypeScript support for seamless on-chain interaction.
- **Multi-version JSON-RPC support**: Compatible with both [legacy](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md) and [new](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) JSON-RPC APIs for broad ecosystem interoperability.
- **Light client support**: Designed to work with light clients such as [Smoldot](https://github.com/smol-dot/smoldot).
- **Native TypeScript for scale codec**: Implements scale codec parsing directly in TypeScript without relying on custom wrappers.
- **Wallet integration**: Works out-of-the-box with [@polkadot/extension-based](https://github.com/polkadot-js/extension?tab=readme-ov-file#api-interface) wallets.
- **Familiar API design**: Similar API style to Polkadot.js for easy and fast migration.

## Installation

To add Dedot to your project, use the following command:

=== "npm"

    ```bash
    npm i dedot
    ```

=== "pnpm"

    ```bash
    pnpm add dedot
    ```

=== "yarn"

    ```bash
    yarn add dedot
    ```

To enable auto-completion/IntelliSense for individual chains, install the [`@dedot/chaintypes`](https://www.npmjs.com/package/@dedot/chaintypes) package as a development dependency:

=== "npm"

    ```bash
    npm i -D @dedot/chaintypes
    ```

=== "pnpm"

    ```bash
    pnpm add -D @dedot/chaintypes
    ```

=== "yarn"

    ```bash
    yarn add -D @dedot/chaintypes
    ```

## Get Started

### Initialize a Client Instance

To connect to and interact with different networks, Dedot provides two client options depending on your needs:

- **[`DedotClient`](https://docs.dedot.dev/clients-and-providers/clients#dedotclient)**: Interacts with chains via the [new JSON-RPC APIs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html).
- **[`LegacyClient`](https://docs.dedot.dev/clients-and-providers/clients#legacyclient)**: Interacts with chains via the [legacy JSON-RPC APIs](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md).

Use the following snippets to connect to Polkadot using `DedotClient`:

=== "WebSocket"

    ```typescript
    import { DedotClient, WsProvider } from 'dedot';
    import type { PolkadotApi } from '@dedot/chaintypes';

    // Initialize providers & clients
    const provider = new WsProvider('wss://rpc.polkadot.io');
    const client = await DedotClient.new<PolkadotApi>(provider);
    ```

=== "Light Client (Smoldot)"

    ```typescript
    import { DedotClient, SmoldotProvider } from 'dedot';
    import type { PolkadotApi } from '@dedot/chaintypes';
    import * as smoldot from 'smoldot';

    // import `polkadot` chain spec to connect to Polkadot
    import { polkadot } from '@substrate/connect-known-chains';

    // Start smoldot instance & initialize a chain
    const client = smoldot.start();
    const chain = await client.addChain({ chainSpec: polkadot });

    // Initialize providers & clients
    const provider = new SmoldotProvider(chain);
    const client = await DedotClient.new<PolkadotApi>(provider);
    ```

If the node doesn't support new JSON-RPC APIs yet, you can connect to the network using the `LegacyClient`, which is built on top of the legacy JSON-RPC APIs.

```typescript
import { LegacyClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await LegacyClient.new<PolkadotApi>(provider);
```

### Enable Type and API Suggestions

It is recommended to specify the `ChainApi` interface (e.g., `PolkadotApi` in the example in the previous section) of the chain you want to interact with. This enables type and API suggestions/autocompletion for that particular chain (via IntelliSense). If you don't specify a `ChainApi` interface, a default `SubstrateApi` interface will be used.

```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi, KusamaApi } from '@dedot/chaintypes';

const polkadotClient = await DedotClient.new<PolkadotApi>(
  new WsProvider('wss://rpc.polkadot.io')
);
const kusamaClient = await DedotClient.new<KusamaApi>(
  new WsProvider('wss://kusama-rpc.polkadot.io')
);
const genericClient = await DedotClient.new(
  new WsProvider('ws://localhost:9944')
);
```

If you don't find the `ChainApi` for the network you're working with in [the list](https://github.com/dedotdev/chaintypes?tab=readme-ov-file#supported-networks), you can generate the `ChainApi` (types and APIs) using the built-in [`dedot` cli](https://docs.dedot.dev/cli).

```bash
# Generate ChainApi interface for Polkadot network via rpc endpoint: wss://rpc.polkadot.io
npx dedot chaintypes -w wss://rpc.polkadot.io
```

Or open a pull request to add your favorite network to the [`@dedot/chaintypes`](https://github.com/dedotdev/chaintypes) repo.

### Read On-Chain Data

Dedot provides several ways to read data from the chain:

- **Access runtime constants**: Use the syntax `client.consts.<pallet>.<constantName>` to inspect runtime constants (parameter types).

    ```typescript
    const ss58Prefix = client.consts.system.ss58Prefix;
    console.log('Polkadot ss58Prefix:', ss58Prefix);
    ```

- **Storage queries**: Use the syntax `client.query.<pallet>.<storgeEntry>` to query on-chain storage.

    ```typescript
    const balance = await client.query.system.account('INSERT_ADDRESS');
    console.log('Balance:', balance.data.free);
    ```

- **Subscribe to storage changes**:

    ```typescript
    const unsub = await client.query.system.number((blockNumber) => {
      console.log(`Current block number: ${blockNumber}`);
    });
    ```

- **Call Runtime APIs**: Use the syntax `client.call.<runtimeApi>.<methodName>` to execute Runtime APIs.

    ```typescript
    const metadata = await client.call.metadata.metadataAtVersion(15);
    console.log('Metadata V15', metadata);
    ```

- **Watch on-chain events**: Use the syntax `client.events.<pallet>.<eventName>` to access pallet events.
    
    ```typescript
    const unsub = await client.events.system.NewAccount.watch((events) => {
      console.log('New Account Created', events);
    });
    ```

### Sign and Send Transactions

Sign the transaction using `IKeyringPair` from Keyring ([`@polkadot/keyring`](https://polkadot.js.org/docs/keyring/start/sign-verify/)) and send the transaction.

```typescript
import { cryptoWaitReady } from '@polkadot/util-crypto';
import { Keyring } from '@polkadot/keyring';
// Setup keyring
await cryptoWaitReady();
const keyring = new Keyring({ type: 'sr25519' });
const alice = keyring.addFromUri('//Alice');
// Send transaction
const unsub = await client.tx.balances
  .transferKeepAlive('INSERT_DEST_ADDRESS', 2_000_000_000_000n)
  .signAndSend(alice, async ({ status }) => {
    console.log('Transaction status', status.type);
    if (status.type === 'BestChainBlockIncluded')
    if (status.type === 'Finalized')`
      );
      await unsub();
    }
  });
```

You can also use `Signer` from wallet extensions:

```typescript
const injected = await window.injectedWeb3['polkadot-js'].enable('My dApp');
const account = (await injected.accounts.get())[0];
const signer = injected.signer;
const unsub = await client.tx.balances
  .transferKeepAlive('INSERT_DEST_ADDRESS', 2_000_000_000_000n)
  .signAndSend(account.address, { signer }, async ({ status }) => {
    console.log('Transaction status', status.type);
    if (status.type === 'BestChainBlockIncluded')
    if (status.type === 'Finalized')`
      );
      await unsub();
    }
  });
```

## Where to Go Next

For more detailed information about Dedot, check the [official documentation](https://dedot.dev/).
