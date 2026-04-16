---
title: Add Smart Contract Functionality
description: Add smart contract capabilities to your Polkadot SDK-based blockchain. Explore PVM, EVM, and Wasm integration for enhanced chain functionality.
categories:
- Parachains
url: https://docs.polkadot.com/parachains/customize-runtime/add-smart-contract-functionality/
word_count: 795
token_estimate: 1532
version_hash: sha256:01f2dfc8d1b4fec97770983610f295c4a8ee948447b961208751c72713f874cf
last_updated: '2026-04-15T03:51:44+00:00'
---

# Add Smart Contract Functionality

## Introduction

When building your custom blockchain with the Polkadot SDK, you can add smart contract capabilities through specialized pallets. These pallets enable users to deploy and execute smart contracts, enhancing your chain's programmability and allowing developers to build decentralized applications on your network.

This guide covers three approaches to adding smart contracts to your blockchain:

- **[`pallet-revive`](#pallet-revive)**: Modern unified solution supporting both PVM and EVM bytecode
- **[Frontier](#frontier)**: Ethereum compatibility layer for Polkadot SDK-based chains
- **[`pallet-contracts`](#pallet-contracts-legacy)**: Wasm smart contract support

## pallet-revive

[`pallet-revive`](https://github.com/paritytech/polkadot-sdk/tree/master/substrate/frame/revive) is the modern smart contract solution for Polkadot SDK-based chains. It provides a unified execution environment that supports both PVM and EVM bytecode through dual execution backends.

### Core Components

**Essential Pallet:**
**[`pallet-revive`](https://github.com/paritytech/polkadot-sdk/tree/master/substrate/frame/revive)** provides the core smart contract execution environment with [PVM](https://github.com/polkadot-developers/polkadot-docs/blob/71e1b51bb42ef55e20c2f3b953db86e8c26cd591/smart-contracts/for-eth-devs/dual-vm-stack.md#upgrade-to-polkavm) and [REVM](https://github.com/polkadot-developers/polkadot-docs/blob/71e1b51bb42ef55e20c2f3b953db86e8c26cd591/smart-contracts/for-eth-devs/dual-vm-stack.md#migrate-from-evm) backends.

**RPC Adapter:**
**[`pallet-revive-eth-rpc`](https://crates.io/crates/pallet-revive-eth-rpc)** adds full Ethereum RPC compatibility for Ethereum tooling integration.

### Supported Languages and Compilers

`pallet-revive` accepts smart contracts from multiple languages and compilation paths:

| Language    | Compiler         | Output Bytecode | Execution Backend |
|-------------|------------------|-----------------|-------------------|
| Solidity    | `resolc`         | PVM             | PVM               |
| Solidity    | `solc`           | EVM             | REVM              |

Any language that can compile to PVM bytecode and utilize `pallet-revive`'s host functions (via [`pallet-revive-uapi`](https://paritytech.github.io/polkadot-sdk/master/pallet_revive_uapi/index.html)) is supported.

### How It Works

**Dual Execution Model:**

1. **PVM Backend**: Executes PVM bytecode with native performance optimization.
2. **REVM Backend**: Implements EVM bytecode for compatibility with existing Ethereum contracts, ensuring seamless migration.

### Key Benefits

- **Unified platform**: Deploys both PVM-optimized and EVM-compatible contracts using a single pallet.
- **Performance**: PVM execution provides improved performance compared to the traditional EVM, leveraging the [RISC-V](https://en.wikipedia.org/wiki/RISC-V) architecture to map instructions to the CPU and requires little transpiling.
- **Ethereum compatibility**: Supports full integration with Ethereum tooling via RPC adapter.

### Implementation Examples

See a real-world implementation in the [Polkadot Hub TestNet](https://github.com/paseo-network/runtimes/blob/c965c42a4e0bc9d1e9cc0a340322bc3b8e347bcf/system-parachains/asset-hub-paseo/src/lib.rs#L1122-L1157) in the Polkadot Fellows repository.

## Frontier

[Frontier](https://github.com/polkadot-evm/frontier) is the Ethereum compatibility layer designed for maximum compatibility with the Ethereum ecosystem. It's the ideal choice when you need seamless integration with existing Ethereum tools, dApps, and infrastructure.

### Integration Options

Frontier offers flexible integration depending on your compatibility needs:

### EVM Execution Only

For basic EVM support using Polkadot SDK native APIs:

- **[`pallet-evm`](https://github.com/polkadot-evm/frontier/tree/master/frame/evm)**: Provides the core EVM execution environment.

This configuration allows EVM contract execution but requires using Polkadot SDK-specific APIs for interaction.

### Full Ethereum Compatibility

For complete Ethereum ecosystem integration with Ethereum RPC support:

- **[`pallet-evm`](https://github.com/polkadot-evm/frontier/tree/master/frame/evm)**: Integrates core EVM execution environment.
- **[`pallet-ethereum`](https://github.com/polkadot-evm/frontier/tree/master/frame/ethereum)**: Emulates Ethereum blocks and handles Ethereum-formatted transactions.
- **[`fc-rpc`](https://github.com/polkadot-evm/frontier/tree/master/client/rpc)**: Provides Ethereum JSON-RPC endpoints.

### Key Benefits

- **Ethereum tooling compatibility**: Full compatibility with MetaMask, Hardhat, Remix, Foundry, and other Ethereum development tools.
- **Minimal-friction migration**: Deployment of existing Ethereum dApps with minimal or no modifications.
- **Native Ethereum formats**: Support for Ethereum transaction formats, signatures, and gas mechanics.
- **Block emulation**: Ethereum-style block structure within Substrate's block production.

### Implementation Examples

Production implementations demonstrate Frontier's capabilities:

- **Moonbeam**: See their implementation of [`pallet-evm`](https://github.com/moonbeam-foundation/moonbeam/blob/9e2ddbc9ae8bf65f11701e7ccde50075e5fe2790/runtime/moonbeam/src/lib.rs#L532) and [`pallet-ethereum`](https://github.com/moonbeam-foundation/moonbeam/blob/9e2ddbc9ae8bf65f11701e7ccde50075e5fe2790/runtime/moonbeam/src/lib.rs#L698).

## pallet-contracts (Legacy)

[`pallet-contracts`](https://docs.rs/pallet-contracts/latest/pallet_contracts/index.html#contracts-pallet) is the original Wasm-based smart contract pallet for Polkadot SDK chains. While still functional, it's considered legacy as development efforts have shifted to `pallet-revive`.

### Implementation Example

For reference, Astar's implementation of [`pallet-contracts`](https://github.com/AstarNetwork/Astar/blob/b6f7a408d31377130c3713ed52941a06b5436402/runtime/astar/src/lib.rs#L693) demonstrates production usage.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Add a Pallet to the Runtime__

    ---

    Learn the step-by-step process for integrating Polkadot SDK pallets into your blockchain's runtime.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/customize-runtime/add-existing-pallets/)

</div>
