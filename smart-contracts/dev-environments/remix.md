---
title: Use the Remix IDE on Polkadot Hub
description: Explore the smart contract development and deployment process on Polkadot Hub using Remix IDE, a visual IDE for blockchain developers.
categories:
- Smart Contracts
- Tooling
url: https://docs.polkadot.com/smart-contracts/dev-environments/remix/
word_count: 632
token_estimate: 1031
version_hash: sha256:16cb8cac3b87b3a89c779432600e707a6766a26daa00f6315b3f90edd32701f8
last_updated: '2026-04-15T03:51:44+00:00'
---

# Remix IDE

## Introduction

[Remix](https://remix.ethereum.org/) is a browser-based IDE that makes it easy to write, compile, and deploy smart contracts without installing any local tools. It’s a great place to experiment, learn, and quickly test contracts on Polkadot. You can generate ERC-20, ERC-721, or other OpenZeppelin-standard contracts with the [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot), then paste the code into Remix to compile and deploy. This page introduces the main parts of the Remix interface and shows how to connect it to Polkadot so you can deploy and interact with contracts directly from your browser.

## Prerequisites

Before getting started, ensure you have:

- A browser with the [MetaMask](https://metamask.io/) extension installed
- MetaMask connected to Polkadot (see the [Wallet Integrations](/polkadot-mkdocs/smart-contracts/integrations/wallets/#metamask) guide for setup steps)

## Access Remix IDE

Navigate to [https://remix.ethereum.org/](https://remix.ethereum.org/). The interface will load with a default workspace containing sample contracts. In this interface, you can access the following:

- **Editor panel**: The main coding area where you write and modify your smart contract files. Supports syntax highlighting, auto-completion, and linting.
- **Terminal**: Shows logs from the compiler, deployment events, transactions, and console.log output. Useful for debugging and tracking execution.
- **Plugin panel**: Displays icons for each of the preloaded plugins, the plugin manager, and the settings menu. You'll see a few icons there for each of the preloaded plugins:

    - **File explorer**: Displays your project workspace. You can create, open, rename, and organize Solidity files, scripts, and folders.
    - **File search**: A quick search tool for finding symbols, functions, or text within your project files.
    - **Solidity compiler**: A plugin that compiles your Solidity contracts. It allows you to select compiler versions, enable optimizations, and view compilation errors or warnings.
    - **Deploy & run transactions**: Used to deploy contracts and interact with them. Allows you to choose an environment (JavaScript VM, injected provider, or custom RPC), deploy contracts, send transactions, and call read/write functions.
    - **Debugger**: Allows you to step through a transaction execution line-by-line. You can inspect variables, stack values, storage slots, and opcodes to understand exactly how your contract behaved during a specific transaction.
    - **Git**: Enables basic Git version control directly inside Remix. You can initialize repositories, view diffs, commit changes, and browse project history without needing an external Git client.

![](/polkadot-mkdocs/images/smart-contracts/dev-environments/remix/remix-01.webp)

## Connect Remix to Polkadot

You can connect Remix to Polkadot from the **Deploy & run transactions** tab in the plugin panel:

1. Switch your MetaMask network to Polkadot. For detailed steps on setting up MetaMask for Polkadot, see the [Wallet Integrations](/polkadot-mkdocs/smart-contracts/integrations/wallets/#metamask) guide.
2. Click on the **Environment** dropdown.
3. Hover over **browser extension**.
4. Select **Injected Provider - MetaMask**.

![](/polkadot-mkdocs/images/smart-contracts/dev-environments/remix/remix-02.webp)

Once connected, Remix will display your MetaMask account address under **Accounts**. To switch accounts, change it in MetaMask—Remix updates automatically.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy a Basic Contract__

    ---

    Ready to start using Remix? Learn how to compile, test, and deploy a basic contract.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/)

-   <span class="badge guide">Guide</span> __Deploy an ERC-20__

    ---

    Walk through deploying a fully-functional ERC-20 to Polkadot Hub using Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/)

-   <span class="badge guide">Guide</span> __Deploy an NFT__

    ---

    Walk through deploying an NFT to Polkadot Hub using Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/)

</div>
