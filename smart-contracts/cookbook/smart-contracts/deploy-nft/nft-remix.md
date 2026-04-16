---
title: Deploy an ERC-721 NFT Using Remix
description: Learn how to deploy an ERC-721 NFT contract to Polkadot Hub using Remix, a browser-based IDE for quick prototyping and learning.
categories:
- Basics
- Smart Contracts
url: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/
word_count: 689
token_estimate: 1167
version_hash: sha256:464fc3ffeecef8527fb0c55638ac6cd79e81dd9ca9d7faca40ac6055cc89f5b1
last_updated: '2026-04-15T03:51:44+00:00'
---

# Deploy an NFT with Remix

## Introduction

Non-Fungible Tokens (NFTs) represent unique digital assets commonly used for digital art, collectibles, gaming, and identity verification.

This guide demonstrates how to deploy an [ERC-721](https://eips.ethereum.org/EIPS/eip-721) NFT contract to [Polkadot Hub](/polkadot-mkdocs/smart-contracts/overview/#smart-contract-development). You'll use [OpenZeppelin's battle-tested NFT implementation](https://github.com/OpenZeppelin/openzeppelin-contracts) and [Remix](https://remix.ethereum.org/), a visual, browser-based environment perfect for rapid prototyping and learning. It requires no local installation and provides an intuitive interface for contract development.

## Prerequisites

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming and [ERC-721 NFT](https://ethereum.org/developers/docs/standards/tokens/erc-721/) standards.
- An EVM-compatible [wallet](/polkadot-mkdocs/smart-contracts/connect/) connected to Polkadot Hub. This example utilizes [MetaMask](https://metamask.io/).
- Test tokens for gas fees (available from the [Polkadot faucet](https://faucet.polkadot.io/)). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.

## Create Your Contract

Follow the steps below to create the ERC-721 contract:

1. Navigate to [Remix IDE](https://remix.ethereum.org/) in your web browser.
2. Select the **Create new file** button under the **contracts** folder, and name your contract `MyNFT.sol`.

    ![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/remix-01.webp)

3. Now, paste the following ERC-721 contract code into `MyNFT.sol`:

    ```solidity title="contracts/MyNFT.sol"
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.20;

    import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract MyNFT is ERC721, Ownable {
        uint256 private _nextTokenId;

        constructor(
            address initialOwner
        ) ERC721("MyToken", "MTK") Ownable(initialOwner) {}

        function safeMint(address to) public onlyOwner {
            uint256 tokenId = _nextTokenId++;
            _safeMint(to, tokenId);
        }
    }
    ```

    !!! tip
        The [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot) was used to generate this example ERC-721 contract. Use it to customize and generate your own ERC-20, ERC-721, or other OpenZeppelin-standard contracts for Polkadot Hub.

## Compile the Contract

Solidity source code compiles into bytecode that can be deployed on the blockchain. During this process, the compiler checks the contract for syntax errors, ensures type safety, and generates the machine-readable instructions needed for blockchain execution.

Ensure your `MyNFT.sol` contract is open in the Remix IDE Editor, and use the following steps to compile:

1. Select the **Solidity Compiler** plugin from the left panel.
2. Select the **Compile MyToken.sol** button.

The **Solidity Compiler** icon will display a green checkmark once the contract compiles successfully. If any issues arise during contract compilation, errors and warnings will appear in the terminal panel at the bottom of the screen.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/remix-02.webp)

## Deploy the Contract

Follow these steps to deploy the contract using Remix:

1. Select **Deploy & Run Transactions** from the left panel.
2. Ensure your MetaMask wallet is connected to Polkadot Hub TestNet, then select the **Environment** dropdown and select **Injected Provider - MetaMask**.

    ![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/remix-03.webp)

3. Configure the contract parameters by entering the address that will own the deployed NFT contract.
4. Select the **Deploy** button to initiate the deployment.
5. Approve the transaction in your MetaMask wallet when prompted.
6. You will see the transaction details in the terminal when the deployment succeeds, including the contract address and deployment transaction hash.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/remix-04.webp)

Once successfully deployed, your contract will appear in the **Deployed Contracts** section, ready for interaction.

Congratulations! You've successfully deployed an ERC-721 NFT contract to Polkadot Hub TestNet using Remix IDE. Consider the following resources to build upon your progress.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an ERC-20__

    ---

    Walk through deploying a fully-functional ERC-20 to Polkadot Hub using Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/)

</div>
