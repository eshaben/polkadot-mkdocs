---
title: Deploy a Basic Contract with Hardhat
description: Learn how to deploy a basic smart contract to Polkadot Hub using Hardhat, ideal for professional workflows that require comprehensive testing and debugging.
categories:
- Smart Contracts
url: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-hardhat/
word_count: 785
token_estimate: 1450
version_hash: sha256:740be52ba59162f962a20aaea6b0ee24b3491668520a6c4a194767398e316e1e
last_updated: '2026-04-15T03:51:44+00:00'
---

# Deploy a Basic Contract with Hardhat

## Introduction

This guide demonstrates how to deploy a basic Solidity smart contract to Polkadot Hub TestNet using [Hardhat](https://hardhat.org/), which provides a comprehensive development environment with built-in testing, debugging, and deployment capabilities. It's ideal for professional development workflows and team projects.

## Prerequisites

Before you begin, ensure you have the following:

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming.
- [Node.js](https://nodejs.org/en/download) v22.13.1 or later installed.
- Test tokens for gas fees, available from the [Polkadot faucet](https://faucet.polkadot.io/). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.
- A wallet with a private key for signing transactions.

## Set Up Your Project

Use the following terminal commands to create a directory and initialize your Hardhat project inside of it:

```bash
mkdir hardhat-deployment
cd hardhat-deployment
npx hardhat@^2.27.0 init
```

## Configure Hardhat

Open `hardhat.config.ts` and update to add `polkadotTestnet` to the `networks` configuration as highlighted in the following example code:

```typescript title='hardhat.config.ts' hl_lines='19-23'
import type { HardhatUserConfig } from 'hardhat/config';

import hardhatToolboxViemPlugin from '@nomicfoundation/hardhat-toolbox-viem';
import { vars } from 'hardhat/config';


const config: HardhatUserConfig = {
  plugins: [hardhatToolboxViemPlugin],
  solidity: {
    version: '0.8.28',
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
  networks: {
    polkadotTestnet: {
      url: 'https://services.polkadothub-rpc.com/testnet',
      chainId: 420420417,
      accounts: [vars.get('PRIVATE_KEY')],
    },
  },
};

export default config;
```

!!! tip
    Visit the Hardhat [Configuration variables](https://v2.hardhat.org/hardhat-runner/docs/guides/configuration-variables) documentation to learn how to use Hardhat to handle your private keys securely.

## Create the Contract

Follow these steps to create your smart contract:

1. Delete the default contract file(s) in the `contracts` directory.

2. Create a new file named `Storage.sol` inside the `contracts` directory.

3. Add the following code to create the `Storage.sol` smart contract:

    ```solidity title="Storage.sol"
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.9;

    contract Storage {
        uint256 private storedNumber;

        function store(uint256 num) public {
            storedNumber = num;
        }

        function retrieve() public view returns (uint256)
    }
    ```

## Compile the Contract

Compile your `Storage.sol` contract using the following command:

```bash
npx hardhat compile
```

You will see a message in the terminal confirming the contract was successfully compiled, similar to the following:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx hardhat compile</span>
  <span data-ty>Downloading solc 0.8.28</span>
  <span data-ty>Downloading solc 0.8.28 (WASM build)</span>
  <span data-ty>Compiled 1 Solidity file with solc 0.8.28 (evm target: cancun)</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
## Deploy the Contract

You are now ready to deploy the contract to your chosen network. This example demonstrates deployment to the Polkadot TestNet. Deploy the contract as follows:

1. Delete the default file(s) inside the `ignition/modules` directory.

2. Create a new file named `Storage.ts` inside the `ignition/modules` directory.

3. Open `ignition/modules/Storage.ts` and add the following code to create your deployment module:

    ```typescript title="ignition/modules/Storage.ts"
    import { buildModule } from '@nomicfoundation/hardhat-ignition/modules';

    export default buildModule('StorageModule', (m) => {
      const storage = m.contract('Storage');
      return { storage };
    });
    ```

4. Deploy your contract to Polkadot Hub TestNet using the following command:

    ```bash
    npx hardhat ignition deploy ignition/modules/Storage.ts --network polkadotTestnet 
    ```

5. Confirm the target deployment network name and chain ID when prompted:

    <div id="termynal" data-termynal markdown>
      <span data-ty="input">npx hardhat ignition deploy ignition/modules/Storage.ts --network polkadotTestnet</span>
      <span data-ty>✔ Confirm deploy to network polkadotTestnet (420420417)? … yes</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Hardhat Ignition 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deploying [ StorageModule ]</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>[ StorageModule ] successfully deployed 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deployed Addresses</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Storage - 0x12345.....</span>
      <span data-ty="input"><span class="file-path"></span></span>
    </div>
Congratulations! You've now deployed a basic smart contract to Polkadot Hub TestNet using Hardhat. Consider the following resources to build upon your progress.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an ERC-20__

    ---

    Walk through deploying a fully-functional ERC-20 to the Polkadot Hub using Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-hardhat/)

-   <span class="badge guide">Guide</span> __Deploy an NFT__

    ---

    Walk through deploying an NFT to the Polkadot Hub using Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-hardhat/)

</div>
