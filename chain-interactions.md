---
title: Chain Interactions Overview
description: Learn how to query data, send transactions, enable cross-chain communication, and manage accounts across the Polkadot ecosystem.
categories:
- Chain Interactions
url: https://docs.polkadot.com/chain-interactions/
word_count: 892
token_estimate: 1607
version_hash: sha256:9dd53129be03032cce7e69533ede6dffdd14628d57afdb8300be260b6504bd45
last_updated: '2026-04-15T03:51:44+00:00'
---

# Chain Interactions

## Introduction

Chain interactions form the foundation of building applications on Polkadot. Whether you're querying on-chain data, executing transactions, enabling cross-chain communication, or managing accounts, understanding how to interact with Polkadot-based chains is essential for application developers.

This section provides comprehensive guidance on the various ways to interact with Polkadot chains, from basic queries to complex cross-chain operations. You'll learn how to:

- Query on-chain state and subscribe to blockchain events.
- Send transactions and manage their lifecycle.
- Enable interoperability between parachains through XCM.
- Manage tokens and perform token operations.
- Create and manage accounts programmatically.

Whether you're building a frontend application, a backend service, or integrating with the Polkadot ecosystem, these guides will equip you with the knowledge and tools to effectively interact with chains across the network.

## Core Interaction Patterns

### Query On-Chain Data

Accessing blockchain state is fundamental to building responsive applications. Polkadot offers several methods to query on-chain data, each suited for different use cases.

- **[SDK integration](/polkadot-mkdocs/chain-interactions/query-data/query-sdks/)**: Programmatically read blockchain state using:

    - [Polkadot API (PAPI)](/polkadot-mkdocs/reference/tools/papi/)
    - [Polkadot.js](/polkadot-mkdocs/reference/tools/polkadot-js-api/)
    - [Dedot](/polkadot-mkdocs/reference/tools/dedot/)
    - [Python Substrate Interface](/polkadot-mkdocs/reference/tools/py-substrate-interface/)
    - [Subxt](/polkadot-mkdocs/reference/tools/subxt/)

- **[REST API access](/polkadot-mkdocs/chain-interactions/query-data/query-rest/)**: Query chain data through standardized REST endpoints for simpler integration.
- **[Runtime API calls](/polkadot-mkdocs/chain-interactions/query-data/runtime-api-calls/)**: Execute runtime APIs directly for specialized queries and operations.

### Send Transactions

Transactions are the primary mechanism for modifying blockchain state. Understanding transaction construction, signing, and submission is crucial for building interactive applications.

- **[Transaction construction](/polkadot-mkdocs/chain-interactions/send-transactions/with-sdks/)**: Build transactions using various SDKs with proper encoding and formatting.
- **[Fee estimation](/polkadot-mkdocs/chain-interactions/send-transactions/calculate-transaction-fees/)**: Calculate transaction fees to ensure sufficient balance and optimize costs.
- **[Multi-token fees](/polkadot-mkdocs/chain-interactions/send-transactions/pay-fees-with-different-tokens/)**: Learn how to pay transaction fees with different tokens on supported chains.

### Send Cross-Chain Transactions

Polkadot enables native cross-chain capabilities through Cross-Consensus Messaging (XCM), allowing chains to securely communicate and transfer assets across the ecosystem.

- **[Transfer assets between parachains](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/transfer-assets-parachains/)**: Understand how to construct and send XCM messages using [ParaSpell XCM SDK](/polkadot-mkdocs/reference/tools/paraspell/) and [Polkadot API (PAPI)](/polkadot-mkdocs/reference/tools/papi/).
- **[Transfer assets into Polkadot](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/transfer-assets-into-polkadot/)**: A step-by-step guide to bridging assets from Ethereum to Polkadot using the [ParaSpell XCM SDK](/polkadot-mkdocs/reference/tools/paraspell/) and [Snowbridge](https://wiki.polkadot.com/learn/learn-snowbridge/).

### Manage Tokens

Polkadot Hub provides a unified platform for managing assets across the ecosystem. Understanding token operations is essential for DeFi applications and multi-chain asset management.

- **[Local asset registration](/polkadot-mkdocs/chain-interactions/token-operations/register-local-asset/)**: Learn how assets created in Asset Hub are registered on the network.
- **[Foregin asset registration](/polkadot-mkdocs/chain-interactions/token-operations/register-foreign-asset/)**: Learn how assets created outside of Asset Hub are registered on the network.
- **[Convert assets](/polkadot-mkdocs/chain-interactions/token-operations/convert-assets/)**: Convert, swap, and manage assets on-chain using the Asset Conversion pallet.
- **Asset Hub integration**: Interact with Polkadot's central asset management hub using [Polkadot.js Apps](https://polkadot.js.org/apps/).

### Manage Accounts

Account management forms the basis of user identity and authentication in blockchain applications. Learn how to create, manage, and query accounts programmatically.

- **[Account creation](/polkadot-mkdocs/chain-interactions/accounts/create-account/)**: Generate accounts using various SDKs in Rust, Python, and JavaScript.
- **[Account queries](/polkadot-mkdocs/chain-interactions/accounts/query-accounts/)**: Retrieve account information including balances, nonces, and metadata.

## Development Tools and SDKs

The Polkadot ecosystem offers a rich set of tools and libraries to facilitate chain interactions:

- **[Polkadot API (PAPI)](/polkadot-mkdocs/reference/tools/papi/)**: Modern, type-safe TypeScript library with full metadata support.
- **[Polkadot.js](/polkadot-mkdocs/reference/tools/polkadot-js-api/)**: Comprehensive JavaScript library with extensive ecosystem support.
- **[Dedot](/polkadot-mkdocs/reference/tools/dedot/)**: Lightweight TypeScript library optimized for performance.
- **[Python Substrate Interface](/polkadot-mkdocs/reference/tools/py-substrate-interface/)**: Polkadot Substrate Interface for streamlined development.
- **[Subxt](/polkadot-mkdocs/reference/tools/subxt/)**: Rust library for building robust substrate-based applications.
- **[Polkadot.js Apps](https://polkadot.js.org/apps/)**: Web-based interface for exploring and interacting with chains.

Each tool has its strengths, and choosing the right one depends on your project requirements, programming language preference, and specific use cases.

## Next Steps

Explore the sections below to dive deeper into specific chain interaction patterns:

- **[Query On-Chain Data](/polkadot-mkdocs/chain-interactions/query-data/query-sdks/)**: Learn to read blockchain state efficiently.
- **[Send Transactions](/polkadot-mkdocs/chain-interactions/send-transactions/with-sdks/)**: Master transaction construction and submission.
- **[Send Cross-Chain Transactions](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/transfer-assets-parachains/)**: Enable cross-chain communication with XCM.
- **[Manage Tokens](/polkadot-mkdocs/chain-interactions/token-operations/register-local-asset/)**: Manage assets across the Polkadot ecosystem.
- **[Manage Accounts](/polkadot-mkdocs/chain-interactions/accounts/create-account/)**: Create and query accounts programmatically.

Each section provides practical examples, code snippets, and comprehensive guides to help you build production-ready applications on Polkadot.
