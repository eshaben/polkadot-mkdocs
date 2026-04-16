---
category: Smart Contracts
description: Developing and deploying smart contracts on Polkadot Hub using Solidity, EVM, PolkaVM, and tools like Hardhat and Foundry.
page_count: 40
token_estimate: 6786
updated: '2026-04-16T19:42:11.755721+00:00'
---

## Accounts in Polkadot Hub Smart Contracts
https://docs.polkadot.com/smart-contracts/for-eth-devs/accounts.md

Bridges Ethereum's 20-byte addresses with Polkadot's 32-byte accounts, enabling seamless interaction while maintaining compatibility with Ethereum tooling.

### Sections
- Introduction `#introduction`
- Address Types and Mappings `#address-types-and-mappings`
- Ethereum to Polkadot Mapping `#ethereum-to-polkadot-mapping`
- Polkadot to Ethereum Mapping `#polkadot-to-ethereum-mapping`
- Interacting with Unmapped Substrate Accounts `#interacting-with-unmapped-substrate-accounts`
- Account Mapping for Native Polkadot Accounts `#account-mapping-for-native-polkadot-accounts`
- Account Registration `#account-registration`
- Fallback Accounts `#fallback-accounts`
- How Fallback Works with Unmapped Accounts `#how-fallback-works-with-unmapped-accounts`
- Contract Address Generation `#contract-address-generation`
- Security Considerations `#security-considerations`

---

## Advanced Functionalities via Precompiles
https://docs.polkadot.com/smart-contracts/precompiles.md

Explores how Polkadot integrates precompiles to run essential functions natively, improving the speed and efficiency of smart contracts on the Hub.

### Sections
- Introduction `#introduction`
- What are Precompiles? `#what-are-precompiles`
- Conclusion `#conclusion`

---

## Block Explorers
https://docs.polkadot.com/smart-contracts/explorers.md

Access block explorers like Subscan, BlockScout, and Routescan to track transactions, analyze contracts, and view on-chain data from smart contracts.

### Sections
- Introduction `#introduction`
- Core Functionality `#core-functionality`
- Available Block Explorers `#available-block-explorers`
- BlockScout `#blockscout`
- Routescan `#routescan`
- Subscan `#subscan`

---

## Connect to Polkadot
https://docs.polkadot.com/smart-contracts/connect.md

Explore how to connect to Polkadot Hub, configure your wallet, and obtain test tokens for developing and testing smart contracts.

### Sections
- Networks Details `#networks-details`
- Test Tokens `#test-tokens`
- Where to Go Next `#where-to-go-next`

---

## Contract Deployment
https://docs.polkadot.com/smart-contracts/for-eth-devs/contract-deployment.md

Compare deployment flows for REVM and PVM-based smart contracts on the Polkadot Hub. Includes single-step REVM flows and PVM's two-step deployment model.

### Sections
- Introduction `#introduction`
- REVM Deployment `#revm-deployment`
- PVM Deployment `#pvm-deployment`
- Standard Contract Deployment `#standard-contract-deployment`
- Two-Step Deployment Model `#two-step-deployment-model`
- Factory Pattern Considerations `#factory-pattern-considerations`
- Migration Strategy for Factory Contracts `#migration-strategy-for-factory-contracts`
- Architecture-Specific Limitations `#architecture-specific-limitations`
- On-Chain Constructors `#on-chain-constructors`
- Gas Estimation vs Actual Consumption `#gas-estimation-vs-actual-consumption`
- Deployment Comparison `#deployment-comparison`
- Conclusion `#conclusion`

---

## Deploy a Basic Contract with Hardhat
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-hardhat.md

Learn how to deploy a basic smart contract to Polkadot Hub using Hardhat, ideal for professional workflows that require comprehensive testing and debugging.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up Your Project `#set-up-your-project`
- Configure Hardhat `#configure-hardhat`
- Create the Contract `#create-the-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy a Basic Contract with Remix IDE
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix.md

Learn how to deploy a basic smart contract to Polkadot Hub using Remix IDE, ideal for rapid prototyping, learning, and visual development.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Locate Your Contract `#locate-your-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy an ERC-20 Using Hardhat
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-hardhat.md

Deploy an ERC-20 token on Polkadot Hub using PVM. This guide covers contract creation, compilation, deployment, and interaction via Hardhat.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up Your Project `#set-up-your-project`
- Configure Hardhat `#configure-hardhat`
- Compile the Contract `#compile-the-contract`
- Test the Contract `#test-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy an ERC-20 Using Remix IDE
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix.md

Deploy an ERC-20 token contract on Polkadot Hub. This guide covers contract creation, compilation, deployment, and interaction via the Remix IDE.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Create Your Contract `#create-your-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Interact with the Contract `#interact-with-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy an ERC-721 NFT Using Remix
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix.md

Learn how to deploy an ERC-721 NFT contract to Polkadot Hub using Remix, a browser-based IDE for quick prototyping and learning.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Create Your Contract `#create-your-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy an ERC-721 Using Hardhat
https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-hardhat.md

Learn how to deploy an ERC-721 NFT contract to Polkadot Hub using Hardhat, a comprehensive development environment with built-in deployment capabilities.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up Your Project `#set-up-your-project`
- Configure Hardhat `#configure-hardhat`
- Create the Contract `#create-the-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy Contracts to Polkadot Hub with Ethers.js
https://docs.polkadot.com/smart-contracts/libraries/ethers-js.md

Learn how to interact with Polkadot Hub using Ethers.js, from compiling and deploying Solidity contracts to interacting with deployed smart contracts.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Project Structure `#project-structure`
- Set Up the Project `#set-up-the-project`
- Install Dependencies `#install-dependencies`
- Set Up the Ethers.js Provider `#set-up-the-ethersjs-provider`
- Compile Contracts `#compile-contracts`
- Sample Storage Smart Contract `#sample-storage-smart-contract`
- Compile the Smart Contract `#compile-the-smart-contract`
- Deploy the Compiled Contract `#deploy-the-compiled-contract`
- Interact with the Contract `#interact-with-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploy Contracts to Polkadot Hub with Web3.js
https://docs.polkadot.com/smart-contracts/libraries/web3-js.md

Learn how to interact with Polkadot Hub using Web3.js, from compiling and deploying Solidity contracts to interacting with deployed smart contracts.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Project Structure `#project-structure`
- Set Up the Project `#set-up-the-project`
- Install Dependencies `#install-dependencies`
- Set Up the Web3 Provider `#set-up-the-web3-provider`
- Compile Contracts `#compile-contracts`
- Sample Storage Smart Contract `#sample-storage-smart-contract`
- Compile the Smart Contract `#compile-the-smart-contract`
- Deploy the Compiled Contract `#deploy-the-compiled-contract`
- Interact with the Contract `#interact-with-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Deploying Uniswap V2 Core on Polkadot
https://docs.polkadot.com/smart-contracts/cookbook/eth-dapps/uniswap-v2/core-v2-pvm.md

Learn how to deploy and test Uniswap V2 Core on Polkadot Hub using Hardhat, bringing AMM-based token swaps to the Polkadot ecosystem.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up the Project `#set-up-the-project`
- Understanding Uniswap V2 Architecture `#understanding-uniswap-v2-architecture`
- Test the Contracts `#test-the-contracts`
- Deploy the Contracts `#deploy-the-contracts`
- Conclusion `#conclusion`

---

## Dual Virtual Machine Stack
https://docs.polkadot.com/smart-contracts/for-eth-devs/dual-vm-stack.md

Compare Polkadot’s dual smart contract VMs—REVM for EVM compatibility and PolkaVM for RISC-V performance, flexibility, and efficiency.

### Sections
- Introduction `#introduction`
- REVM Backend `#revm-backend`
- Key Benefits `#key-benefits`
- How It Works `#how-it-works`
- Architecture `#architecture`
- Revive Pallet `#revive-pallet`
- Alternative: PVM Backend `#alternative-pvm-backend`
- Where To Go Next `#where-to-go-next`

---

## Ethereum-Native Precompiles
https://docs.polkadot.com/smart-contracts/precompiles/eth-native.md

General overview of Ethereum-native precompiles in Polkadot Hub’s Revive pallet, including usage basics and details on standard precompiles for smart contracts.

### Sections
- Introduction `#introduction`
- How to Use Precompiles `#how-to-use-precompiles`
- Standard Precompiles in Polkadot Hub `#standard-precompiles-in-polkadot-hub`
- Conclusion `#conclusion`

---

## EVM vs PVM
https://docs.polkadot.com/smart-contracts/for-eth-devs/evm-vs-pvm.md

Compares EVM and PVM, highlighting key architectural differences, gas models, memory management, and account handling while ensuring Solidity compatibility.

### Sections
- Introduction `#introduction`
- Core Virtual Machine Architecture `#core-virtual-machine-architecture`
- High-Level Architecture Comparison `#high-level-architecture-comparison`
- Gas Model `#gas-model`
- Dynamic Gas Value Scaling `#dynamic-gas-value-scaling`
- Multi-Dimensional Resource Metering `#multi-dimensional-resource-metering`
- Memory Management `#memory-management`
- Current Memory Limits `#current-memory-limits`
- Account Management - Existential Deposit `#account-management-existential-deposit`
- Account Management Comparison `#account-management-comparison`
- Contract Deployment `#contract-deployment`
- Solidity and YUL IR Translation Incompatibilities `#solidity-and-yul-ir-translation-incompatibilities`
- Contract Code Structure `#contract-code-structure`
- Solidity-Specific Differences `#solidity-specific-differences`
- YUL Function Translation Differences `#yul-function-translation-differences`
- Unsupported Operations `#unsupported-operations`
- Compilation Pipeline Considerations `#compilation-pipeline-considerations`
- Memory Pointer Limitations `#memory-pointer-limitations`

---

## Gas Model on the Polkadot Hub
https://docs.polkadot.com/smart-contracts/for-eth-devs/gas-model.md

Learn how gas estimation, pricing, and weight mapping work in the Polkadot Hub.

### Sections
- Introduction `#introduction`
- Understanding Resources in the Polkadot Hub `#understanding-resources-in-the-polkadot-hub`
- Gas vs Weight `#gas-vs-weight`
- How Gas Estimation Works `#how-gas-estimation-works`
- Dynamic Gas Pricing `#dynamic-gas-pricing`
- Transaction Execution Flow `#transaction-execution-flow`
- Conclusion `#conclusion`

---

## Get Started with Smart Contracts
https://docs.polkadot.com/smart-contracts/get-started.md

Practical examples for building and deploying smart contracts on Polkadot Hub, from connecting and tooling to deployment, integrations, and precompiles.

### Sections
- Quick Starts `#quick-starts`
- Build and Test Locally `#build-and-test-locally`
- Ethereum Tool Differences on Polkadot EVM `#ethereum-tool-differences-on-polkadot-evm`
- Ethereum Developer Resources `#ethereum-developer-resources`
- Cookbook: Hands-on Tutorials `#cookbook-hands-on-tutorials`
- Libraries `#libraries`
- Integrations `#integrations`
- Precompiles `#precompiles`

---

## Get Tokens from the Official Faucet
https://docs.polkadot.com/smart-contracts/faucet.md

Learn how to obtain test tokens from Polkadot faucets for development and testing purposes across different networks.

### Sections
- Get Test Tokens `#get-test-tokens`
- Things to Consider `#things-to-consider`
- Using Your Test Tokens `#using-your-test-tokens`

---

## Interact with the ERC20 Precompile
https://docs.polkadot.com/smart-contracts/precompiles/erc20.md

Learn how to use the ERC20 precompile to interact with assets from the assets pallet using standard ERC20 token interfaces in your smart contracts.

### Sections
- Introduction `#introduction`
- Supported Asset Types `#supported-asset-types`
- Trust-Backed Assets `#trust-backed-assets`
- Foreign Assets `#foreign-assets`
- Pool Assets `#pool-assets`
- Precompile Interface `#precompile-interface`
- Query Functions `#query-functions`
- Get Total Supply `#get-total-supply`
- Get Balance `#get-balance`
- Check Allowance `#check-allowance`
- Token Operations `#token-operations`
- Transfer Tokens `#transfer-tokens`
- Approve Spending `#approve-spending`
- Transfer From `#transfer-from`
- Common Trust-Backed Asset IDs `#common-trust-backed-asset-ids`
- Interact with the ERC20 Precompile `#interact-with-the-erc20-precompile`
- Conclusion `#conclusion`
- Reference `#reference`

---

## Interact with the Storage Precompile
https://docs.polkadot.com/smart-contracts/precompiles/storage.md

Learn how to use the Storage precompile for low-level contract storage access, including reads, writes, partial reads, key inspection, and storage management.

### Sections
- Introduction `#introduction`
- Precompile Interface `#precompile-interface`
- Reading Storage `#reading-storage`
- Read Full Value `#read-full-value`
- Read Partial Value `#read-partial-value`
- Read Value Prefix `#read-value-prefix`
- Writing Storage `#writing-storage`
- Write Value `#write-value`
- Remove Value `#remove-value`
- Storage Inspection `#storage-inspection`
- Check Key Existence `#check-key-existence`
- Get Value Length `#get-value-length`
- Interact with the Storage Precompile `#interact-with-the-storage-precompile`
- Conclusion `#conclusion`
- Reference `#reference`

---

## Interact with the System Precompile
https://docs.polkadot.com/smart-contracts/precompiles/system.md

Learn how to use the System precompile to access core runtime functionality, cryptographic operations, and system utilities from your smart contracts.

### Sections
- Introduction `#introduction`
- Precompile Interface `#precompile-interface`
- Cryptographic Operations `#cryptographic-operations`
- Compute BLAKE2-256 Hash `#compute-blake2-256-hash`
- Compute BLAKE2-128 Hash `#compute-blake2-128-hash`
- Verify SR25519 Signature `#verify-sr25519-signature`
- Convert ECDSA Public Key to Ethereum Address `#convert-ecdsa-public-key-to-ethereum-address`
- Account Management `#account-management`
- Convert Address to Account ID `#convert-address-to-account-id`
- Runtime Queries `#runtime-queries`
- Check if Caller is Origin `#check-if-caller-is-origin`
- Check if Caller is Root `#check-if-caller-is-root`
- Get Minimum Balance `#get-minimum-balance`
- Get Own Code Hash `#get-own-code-hash`
- Get Weight Left `#get-weight-left`
- Contract Lifecycle `#contract-lifecycle`
- Terminate Contract `#terminate-contract`
- Interact with the System Precompile `#interact-with-the-system-precompile`
- Conclusion `#conclusion`
- Reference `#reference`

---

## Interact with the XCM Precompile
https://docs.polkadot.com/smart-contracts/precompiles/xcm.md

Learn how to use the XCM precompile to send cross-chain messages, execute XCM instructions, and estimate costs from your smart contracts.

### Sections
- Introduction `#introduction`
- Precompile Interface `#precompile-interface`
- Interact with the XCM Precompile `#interact-with-the-xcm-precompile`
- Weigh a Message `#weigh-a-message`
- Execute a Message `#execute-a-message`
- Send a Message `#send-a-message`
- Cross Contract Calls `#cross-contract-calls`
- Conclusion `#conclusion`
- Next Steps `#next-steps`

---

## Local Development Node
https://docs.polkadot.com/smart-contracts/dev-environments/local-dev-node.md

Follow this step-by-step guide to install a Revive Dev node and ETH-RPC adapter for smart contract development in a local environment.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Install the Revive Dev Node and ETH-RPC Adapter `#install-the-revive-dev-node-and-eth-rpc-adapter`
- Run the Local Node `#run-the-local-node`

---

## Smart Contracts Cookbook
https://docs.polkadot.com/smart-contracts/cookbook.md

Explore our full collection of tutorials and guides to learn step-by-step how to build, deploy, and work with smart contracts on Polkadot.

### Sections
- Get Tokens from the Faucet `#get-tokens-from-the-faucet`
- EVM Smart Contracts `#evm-smart-contracts`
- Port Ethereum DApps `#port-ethereum-dapps`

---

## Smart Contracts Overview
https://docs.polkadot.com/smart-contracts/overview.md

Learn about smart contract development on Polkadot Hub with native PVM support, dual-VM execution, and seamless cross-chain capabilities.

### Sections
- Introduction `#introduction`
- Why Build on Polkadot Hub `#why-build-on-polkadot-hub`
- Ethereum Compatibility `#ethereum-compatibility`
- Performance Options `#performance-options`
- Cross-VM  & Cross-Chain Capabilities `#cross-vm-cross-chain-capabilities`
- Other Smart Contract Environments `#other-smart-contract-environments`
- Next Steps `#next-steps`

---

## Transactions and Fees on Asset Hub
https://docs.polkadot.com/smart-contracts/for-eth-devs/blocks-transactions-fees.md

Explore how Asset Hub smart contracts handle blocks, transactions, and fees with EVM compatibility, supporting various Ethereum transaction types.

### Sections
- Introduction `#introduction`
- Smart Contract Blocks `#smart-contract-blocks`
- Smart Contract Transactions `#smart-contract-transactions`
- EVM Transaction Types `#evm-transaction-types`
- Fees and Gas `#fees-and-gas`
- Gas Model Overview `#gas-model-overview`
- Fee Components `#fee-components`
- Gas Calculation and Conversion `#gas-calculation-and-conversion`

---

## Uniswap V2 Core with EVM on Polkadot
https://docs.polkadot.com/smart-contracts/cookbook/eth-dapps/uniswap-v2/core-v2.md

Deploy and test unmodified Uniswap V2 Core contracts on Polkadot Hub using standard Hardhat and TypeScript with the EVM execution path.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up the Project `#set-up-the-project`
- Configure Secure Key Management `#configure-secure-key-management`
- Uniswap V2 Core Architecture `#uniswap-v2-core-architecture`
- Test the Contracts `#test-the-contracts`
- Deploy the Contracts `#deploy-the-contracts`
- Deploy to the Local Node `#deploy-to-the-local-node`
- Deploy to the Polkadot Hub TestNet `#deploy-to-the-polkadot-hub-testnet`
- Where to Go Next `#where-to-go-next`

---

## Uniswap V2 on Polkadot
https://docs.polkadot.com/smart-contracts/cookbook/eth-dapps/uniswap-v2.md

Deploy and test the full Uniswap V2 stack on Polkadot Hub — from core AMM contracts to periphery router interfaces — using standard Ethereum tooling.

### Sections
- Tutorials `#tutorials`

---

## Uniswap V2 Periphery with EVM on Polkadot
https://docs.polkadot.com/smart-contracts/cookbook/eth-dapps/uniswap-v2/periphery-v2.md

Deploy and test unmodified Uniswap V2 Periphery Router contracts on Polkadot Hub using standard Hardhat and TypeScript with the EVM execution path.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up the Project `#set-up-the-project`
- Configure Secure Key Management `#configure-secure-key-management`
- Uniswap V2 Periphery Architecture `#uniswap-v2-periphery-architecture`
- Test the Contracts `#test-the-contracts`
- Deploy the Contracts `#deploy-the-contracts`
- Where to Go Next `#where-to-go-next`

---

## Use Foundry with Polkadot Hub
https://docs.polkadot.com/smart-contracts/dev-environments/foundry.md

Learn how to create, compile, test, deploy, and verify smart contracts on Polkadot Hub using Foundry, a fast and portable Ethereum toolkit.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Install Foundry `#install-foundry`
- Initialize a Foundry Project `#initialize-a-foundry-project`
- Compile Contracts `#compile-contracts`
- Configure Foundry for Polkadot Hub `#configure-foundry-for-polkadot-hub`
- Available Networks and RPC Endpoints `#available-networks-and-rpc-endpoints`
- Deploy a Contract `#deploy-a-contract`
- Set Up Environment Variables `#set-up-environment-variables`
- Deploy Using Forge `#deploy-using-forge`
- Deploy Using Scripts `#deploy-using-scripts`
- Verify a Contract `#verify-a-contract`
- Basic Verification `#basic-verification`
- Verification with Constructor Arguments `#verification-with-constructor-arguments`
- Interact with Contracts `#interact-with-contracts`
- Using Cast `#using-cast`
- Using Forge Scripts `#using-forge-scripts`
- Run Tests `#run-tests`
- Additional Resources `#additional-resources`
- Where to Go Next `#where-to-go-next`

---

## Use Hardhat with Polkadot Hub
https://docs.polkadot.com/smart-contracts/dev-environments/hardhat-polkadot.md

Learn how to set up, configure, and use Hardhat Polkadot to create, compile, test, and deploy Solidity smart contracts on Polkadot Hub using the PVM runtime.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Initialize a Hardhat Project `#initialize-a-hardhat-project`
- Compile Your Contract `#compile-your-contract`
- Test Your Contract Locally `#test-your-contract-locally`
- Start a Local Node `#start-a-local-node`
- Run Tests `#run-tests`
- Deploy to Local Node `#deploy-to-local-node`
- Deploy to Polkadot Hub TestNet `#deploy-to-polkadot-hub-testnet`
- Common Issues `#common-issues`
- Where to Go Next `#where-to-go-next`

---

## Use Hardhat with Polkadot Hub
https://docs.polkadot.com/smart-contracts/dev-environments/hardhat.md

Learn how to create, compile, test, and deploy smart contracts on Polkadot Hub using Hardhat, a powerful development environment for blockchain developers.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Initialize a Hardhat Project `#initialize-a-hardhat-project`
- Configure Hardhat for Polkadot Hub `#configure-hardhat-for-polkadot-hub`
- Verify a Contract `#verify-a-contract`
- Install the Verification Plugin `#install-the-verification-plugin`
- Add Verification Config `#add-verification-config`
- Basic Verification `#basic-verification`
- Verification with Constructor Arguments `#verification-with-constructor-arguments`
- Where to Go Next `#where-to-go-next`

---

## Use the Remix IDE on Polkadot Hub
https://docs.polkadot.com/smart-contracts/dev-environments/remix.md

Explore the smart contract development and deployment process on Polkadot Hub using Remix IDE, a visual IDE for blockchain developers.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Access Remix IDE `#access-remix-ide`
- Connect Remix to Polkadot `#connect-remix-to-polkadot`
- Where to Go Next `#where-to-go-next`

---

## viem for Polkadot Hub Smart Contracts
https://docs.polkadot.com/smart-contracts/libraries/viem.md

This guide covers deploying and interacting with contracts on Polkadot Hub using viem, a TypeScript library for Ethereum-compatible chains.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Project Structure `#project-structure`
- Set Up the Project `#set-up-the-project`
- Install Dependencies `#install-dependencies`
- Initialize Project `#initialize-project`
- Set Up the Chain Configuration `#set-up-the-chain-configuration`
- Set Up the viem Client `#set-up-the-viem-client`
- Set Up a Wallet `#set-up-a-wallet`
- Sample Smart Contract `#sample-smart-contract`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Interact with the Contract `#interact-with-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Wagmi for Polkadot Hub Smart Contracts
https://docs.polkadot.com/smart-contracts/libraries/wagmi.md

Learn how to use Wagmi React Hooks to fetch and interact with smart contracts on Polkadot Hub for seamless dApp integration.

### Sections
- Introduction `#introduction`
- Set Up the Project `#set-up-the-project`
- Install Dependencies `#install-dependencies`
- Configure Wagmi for Polkadot Hub `#configure-wagmi-for-polkadot-hub`
- Set Up the Wagmi Provider `#set-up-the-wagmi-provider`
- Connect a Wallet `#connect-a-wallet`
- Fetch Blockchain Data `#fetch-blockchain-data`
- Interact with Deployed Contract `#interact-with-deployed-contract`
- Integrate Components `#integrate-components`
- Where to Go Next `#where-to-go-next`

---

## Wallets for Polkadot Hub
https://docs.polkadot.com/smart-contracts/integrations/wallets.md

Comprehensive guide to connecting and managing wallets for Polkadot Hub, covering step-by-step instructions for interacting with the ecosystem.

### Sections
- Introduction `#introduction`
- Connect Your Wallet `#connect-your-wallet`
- MetaMask `#metamask`
- Talisman `#talisman`
- Conclusion `#conclusion`

---

## Web3.py
https://docs.polkadot.com/smart-contracts/libraries/web3-py.md

Learn how to interact with Polkadot Hub using the Web3 python library, deploying Solidity contracts, and interacting with deployed smart contracts.

### Sections
- Introduction `#introduction`
- Set Up the Project `#set-up-the-project`
- Set Up the Web3 Provider `#set-up-the-web3-provider`
- Compile Contracts `#compile-contracts`
- Sample Storage Smart Contract `#sample-storage-smart-contract`
- Compile the Smart Contract `#compile-the-smart-contract`
- Contract Deployment `#contract-deployment`
- Interact with the Contract `#interact-with-the-contract`
- Where to Go Next `#where-to-go-next`

---

## Zero to Hero Smart Contract DApp
https://docs.polkadot.com/smart-contracts/cookbook/dapps/zero-to-hero.md

Learn how to build a decentralized application on Polkadot Hub using Viem and Next.js by creating a simple dApp that interacts with a smart contract.

### Sections
- Prerequisites `#prerequisites`
- Project Overview `#project-overview`
- Create and Deploy the Storage Contract `#create-and-deploy-the-storage-contract`
- Set Up Hardhat Project `#set-up-hardhat-project`
- Create the Storage Contract `#create-the-storage-contract`
- Configure Hardhat for Polkadot Hub `#configure-hardhat-for-polkadot-hub`
- Compile the Contract `#compile-the-contract`
- Deploy the Contract `#deploy-the-contract`
- Export the Contract ABI `#export-the-contract-abi`
- Set Up the DApp Project `#set-up-the-dapp-project`
- Install Dependencies `#install-dependencies`
- Connect to Polkadot Hub `#connect-to-polkadot-hub`
- Set Up the Smart Contract Interface `#set-up-the-smart-contract-interface`
- Create the Wallet Connection Component `#create-the-wallet-connection-component`
- Create the Read Contract Component `#create-the-read-contract-component`
- Create the Write Contract Component `#create-the-write-contract-component`
- How It Works `#how-it-works`
- Wallet Connection `#wallet-connection`
- Data Reads `#data-reads`
- Data Writes `#data-writes`
- Conclusion `#conclusion`
- Where to Go Next `#where-to-go-next`
