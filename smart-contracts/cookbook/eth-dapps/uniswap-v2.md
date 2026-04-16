---
title: Uniswap V2 on Polkadot
description: Deploy and test the full Uniswap V2 stack on Polkadot Hub — from core AMM contracts to periphery router interfaces — using standard Ethereum tooling.
categories:
- Smart Contracts
- Tooling
url: https://docs.polkadot.com/smart-contracts/cookbook/eth-dapps/uniswap-v2/
word_count: 177
token_estimate: 270
version_hash: sha256:49ca871afae1f930ba7ad22857cfbbc5ba1e1d14cd6eb9083b5a5b23a6275fb3
last_updated: '2026-04-15T03:51:44+00:00'
---

# Uniswap V2 on Polkadot

[Uniswap V2](https://docs.uniswap.org/contracts/v2/overview) is one of the most widely deployed decentralized exchange protocols. Its Automated Market Maker (AMM) architecture enables permissionless token swaps, liquidity provision, and price discovery for any ERC-20 token pair.

The Uniswap V2 stack is split into two layers:

- **V2 Core**: The Factory and Pair contracts that implement the fundamental AMM logic, constant product pricing (x * y = k), and liquidity pool management.
- **V2 Periphery**: The Router contracts that provide user-facing functions for adding and removing liquidity, executing token swaps with slippage protection, and wrapping native ETH through WETH.

Polkadot Hub supports deploying these contracts with no modifications. The tutorials below walk you through deploying, testing, and interacting with each layer.

## Tutorials

<div class="grid cards" markdown>

-   <span class="badge tutorial">Tutorial</span> __V2 Core__

    ---

    Deploy unmodified Uniswap V2 Factory and Pair contracts on Polkadot Hub using Hardhat. Supports both EVM and PVM execution paths.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/eth-dapps/uniswap-v2/core-v2/)

</div>
