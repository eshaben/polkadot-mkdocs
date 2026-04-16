---
title: Deploy a Basic Contract with Remix IDE
description: Learn how to deploy a basic smart contract to Polkadot Hub using Remix IDE, ideal for rapid prototyping, learning, and visual development.
categories:
- Smart Contracts
url: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/
word_count: 547
token_estimate: 875
version_hash: sha256:bcb307ecb9b90a2564be8a53becad09cc82323aec5b3580fae5075f902bdf311
last_updated: '2026-04-15T03:51:44+00:00'
---

# Deploy a Basic Contract with Remix IDE

## Introduction

This guide demonstrates how to deploy a basic Solidity smart contract to Polkadot Hub using [Remix IDE](https://remix.ethereum.org/), which offers a visual, browser-based environment perfect for rapid prototyping and learning. It requires no local installation and provides an intuitive interface for contract development.

## Prerequisites

Before you begin, ensure you have the following:

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming.
- An EVM-compatible [wallet](/polkadot-mkdocs/smart-contracts/connect/) connected to Polkadot Hub. This example utilizes [MetaMask](https://metamask.io/).
- Test tokens for gas fees, available from the [Polkadot faucet](https://faucet.polkadot.io/). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.

## Locate Your Contract

This guide uses a default contract contract provided by Remix IDE. Follow these steps to locate the contract in Remix:

1. Navigate to [Remix IDE](https://remix.ethereum.org/) in your web browser.

2. Once the default workspace loads, locate the `contracts` folder. Inside `contracts`, locate the `Storage.sol` file which you will use as your sample contract throughout this guide.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/remix-01.webp)

## Compile the Contract

Solidity source code compiles into bytecode that can be deployed on the blockchain. During this process, the compiler checks the contract for syntax errors, ensures type safety, and generates the machine-readable instructions needed for blockchain execution. 

Ensure your `Storage.sol` contract is open in the Remix IDE editor, and use the following steps to compile:

1. Select the **Solidity Compiler** plugin from the left panel.
2. Select the **Compile Storage.sol** button.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/remix-02.webp)

The **Solidity Compiler** icon will display a green checkmark once the contract compiles successfully. If any issues arise during contract compilation, errors and warnings will appear in the terminal panel at the bottom of the screen.

## Deploy the Contract

Follow these steps to deploy the contract using Remix: 

1. Select **Deploy & Run Transactions** from the left panel.
2. Ensure your MetaMask wallet is connected to Polkadot Hub TestNet, then select the **Environment** dropdown and select **Injected Provider - MetaMask**.
3. Select the **Deploy** button to initiate the deployment.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/remix-03.webp)

When prompted, approve the transaction in your MetaMask wallet. After the deployment succeeds, the terminal will display the transaction details, including the contract address and transaction hash, and your contract will appear in the **Deployed Contracts** section.

Congratulations! You've successfully deployed a basic smart contract to Polkadot Hub TestNet using Remix IDE. Consider the following resources to build upon your progress.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an ERC-20__

    ---

    Walk through deploying a fully-functional ERC-20 to Polkadot Hub using Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/)

-   <span class="badge guide">Guide</span> __Deploy an NFT__

    ---

    Walk through deploying an NFT to Polkadot Hub using Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/)        

</div>
