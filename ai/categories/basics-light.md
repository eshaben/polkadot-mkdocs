---
category: Basics
description: Polkadot overview, core concepts, and getting started guides for new developers.
page_count: 32
token_estimate: 5439
updated: '2026-04-16T19:42:11.755721+00:00'
---

## Blocks
https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/blocks.md

Understand how blocks are produced, validated, and imported in Polkadot SDK-based blockchains, covering initialization, finalization, and authoring processes.

### Sections
- Introduction `#introduction`
- What is a Block? `#what-is-a-block`
- Block Production `#block-production`
- Initialize Block `#initialize-block`
- Finalize Block `#finalize-block`
- Block Authoring and Import `#block-authoring-and-import`
- Block Import Queue `#block-import-queue`
- Additional Resources `#additional-resources`

---

## Chain Data
https://docs.polkadot.com/reference/parachains/chain-data.md

Learn how to expose and utilize chain data for blockchain applications. Discover runtime metadata, RPC APIs, and tools for efficient development.

### Sections
- Introduction `#introduction`
- Application Development `#application-development`
- Understand Metadata `#understand-metadata`
- Expose Runtime Information as Metadata `#expose-runtime-information-as-metadata`
- Generate Metadata `#generate-metadata`
- Retrieve Runtime Metadata `#retrieve-runtime-metadata`
- Use Polkadot.js `#use-polkadotjs`
- Use Curl `#use-curl`
- Use Subxt `#use-subxt`
- Client Applications and Metadata `#client-applications-and-metadata`
- Metadata Format `#metadata-format`
- Pallets `#pallets`
- Extrinsic `#extrinsic`
- Included RPC APIs `#included-rpc-apis`
- Additional Resources `#additional-resources`

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

## Create an Account
https://docs.polkadot.com/chain-interactions/accounts/create-account.md

Step-by-step guide to creating Polkadot accounts using different programming languages and libraries, including JavaScript, Python, and Rust examples.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Use JavaScript/TypeScript `#use-javascripttypescript`
- Python `#python`
- Rust `#rust`
- Where to Go Next `#where-to-go-next`

---

## Cryptography
https://docs.polkadot.com/reference/parachains/cryptography.md

A concise guide to cryptography in blockchain, covering hash functions, encryption types, digital signatures, and elliptic curve applications.

### Sections
- Introduction `#introduction`
- Hash Functions `#hash-functions`
- Key Properties of Hash Functions `#key-properties-of-hash-functions`
- Blake2 `#blake2`
- Types of Cryptography `#types-of-cryptography`
- Symmetric Cryptography `#symmetric-cryptography`
- Asymmetric Cryptography `#asymmetric-cryptography`
- Trade-offs and Compromises `#trade-offs-and-compromises`
- Digital Signatures `#digital-signatures`
- Example of Creating a Digital Signature `#example-of-creating-a-digital-signature`
- Elliptic Curve `#elliptic-curve`
- Various Implementations `#various-implementations`

---

## Data Encoding
https://docs.polkadot.com/reference/parachains/data-encoding.md

SCALE codec enables fast, efficient data encoding, ideal for resource-constrained environments like Wasm, supporting custom types and compact encoding.

### Sections
- Introduction `#introduction`
- SCALE Codec `#scale-codec`
- Encode `#encode`
- Decode `#decode`
- CompactAs `#compactas`
- HasCompact `#hascompact`
- EncodeLike `#encodelike`
- Data Types `#data-types`
- Encode and Decode Rust Trait Implementations `#encode-and-decode-rust-trait-implementations`
- SCALE Codec Libraries `#scale-codec-libraries`

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

## Get Started with Parachain Development
https://docs.polkadot.com/parachains/get-started.md

Practical examples and tutorials for building and deploying Polkadot parachains, covering everything from launch to customization and cross-chain messaging.

### Sections
- Quick Start Guides `#quick-start-guides`
- Launch a Simple Parachain `#launch-a-simple-parachain`
- Customize Your Runtime `#customize-your-runtime`
- Pallet Development `#pallet-development`
- Testing `#testing`
- Runtime Upgrades and Maintenance `#runtime-upgrades-and-maintenance`
- Interoperability `#interoperability`
- Integrations `#integrations`
- Additional Resources `#additional-resources`

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

## Get Started with XCM
https://docs.polkadot.com/parachains/interoperability/get-started.md

Unlock blockchain interoperability with XCM — Polkadot's Cross-Consensus Messaging format for cross-chain interactions.

### Sections
- Introduction `#introduction`
- Messaging Format `#messaging-format`
- The Four Principles of XCM `#the-four-principles-of-xcm`
- The XCM Tech Stack `#the-xcm-tech-stack`
- Core Functionalities of XCM `#core-functionalities-of-xcm`
- XCM Example `#xcm-example`
- Overview `#overview`

---

## Install Polkadot SDK
https://docs.polkadot.com/parachains/install-polkadot-sdk.md

Install all required Polkadot SDK dependencies, set up the SDK itself, and verify that it runs correctly on your machine.

### Sections
- Install Dependencies: macOS `#install-dependencies-macos`
- Before You Begin {: #before-you-begin-mac-os } `#before-you-begin-before-you-begin-mac-os`
- Install Command Line Tools `#install-command-line-tools`
- Install Homebrew `#install-homebrew`
- Support for Apple Silicon `#support-for-apple-silicon`
- Install Required Packages and Rust {: #install-required-packages-and-rust-mac-os } `#install-required-packages-and-rust-install-required-packages-and-rust-mac-os`
- Install Dependencies: Linux `#install-dependencies-linux`
- Before You Begin {: #before-you-begin-linux } `#before-you-begin-before-you-begin-linux`
- Install Required Packages and Rust {: #install-required-packages-and-rust-linux } `#install-required-packages-and-rust-install-required-packages-and-rust-linux`
- Install Dependencies: Windows (WSL) `#install-dependencies-windows-wsl`
- Before You Begin {: #before-you-begin-windows-wls } `#before-you-begin-before-you-begin-windows-wls`
- Set Up Windows Subsystem for Linux `#set-up-windows-subsystem-for-linux`
- Install Required Packages and Rust {: #install-required-packages-and-rust-windows-wls } `#install-required-packages-and-rust-install-required-packages-and-rust-windows-wls`
- Build the Polkadot SDK `#build-the-polkadot-sdk`
- Clone the Polkadot SDK `#clone-the-polkadot-sdk`
- Compile the Polkadot SDK `#compile-the-polkadot-sdk`
- Verify the Build `#verify-the-build`
- Optional: Run the Kitchensink Node `#optional-run-the-kitchensink-node`
- Run the Kitchensink Node in Development Mode `#run-the-kitchensink-node-in-development-mode`
- Interact with the Kitchensink Node `#interact-with-the-kitchensink-node`
- Where to Go Next `#where-to-go-next`

---

## Interoperability
https://docs.polkadot.com/reference/parachains/interoperability.md

Explore the importance of interoperability in the Polkadot ecosystem, covering XCM, bridges, and cross-chain communication.

### Sections
- Introduction `#introduction`
- Why Interoperability Matters `#why-interoperability-matters`
- Key Mechanisms for Interoperability `#key-mechanisms-for-interoperability`
- Cross-Consensus Messaging (XCM): The Backbone of Communication `#cross-consensus-messaging-xcm-the-backbone-of-communication`
- Bridges: Connecting External Networks `#bridges-connecting-external-networks`
- The Polkadot Advantage `#the-polkadot-advantage`
- Looking Ahead `#looking-ahead`

---

## Networks
https://docs.polkadot.com/reference/parachains/networks.md

Explore Polkadot's networks, with Paseo as the official Polkadot TestNet for parachain and dApp development, plus Kusama and Westend for specialized use cases.

### Sections
- Introduction `#introduction`
- Network Overview `#network-overview`
- Recommended Development Path `#recommended-development-path`
- Alternative Networks `#alternative-networks`
- Polkadot MainNet `#polkadot-mainnet`
- Polkadot TestNet (Paseo) `#polkadot-testnet-paseo`
- Other Networks `#other-networks`
- Kusama Network `#kusama-network`
- Westend `#westend`
- Local Test Networks `#local-test-networks`
- Zombienet `#zombienet`
- Chopsticks `#chopsticks`

---

## Node and Runtime
https://docs.polkadot.com/reference/parachains/node-and-runtime.md

Learn how Polkadot SDK-based nodes function, how the client and runtime are separated, and how they communicate using SCALE-encoded data.

### Sections
- Introduction `#introduction`
- Architectural Principles `#architectural-principles`
- Advantages of this Architecture `#advantages-of-this-architecture`
- Node (Client) `#node-client`
- Runtime `#runtime`
- Characteristics `#characteristics`
- Key Functions `#key-functions`
- Communication Between Node and Runtime `#communication-between-node-and-runtime`
- Runtime APIs `#runtime-apis`
- Host Functions `#host-functions`

---

## On-Chain Governance Overview
https://docs.polkadot.com/reference/governance.md

Discover Polkadot’s cutting-edge OpenGov system, enabling transparent, decentralized decision-making through direct democracy and flexible governance tracks.

### Sections
- Introduction `#introduction`
- Governance Evolution `#governance-evolution`
- OpenGov Key Features `#opengov-key-features`
- Origins and Tracks `#origins-and-tracks`
- Referendums `#referendums`
- Vote on Referendums `#vote-on-referendums`
- Delegate Voting Power `#delegate-voting-power`
- Cancel a Referendum `#cancel-a-referendum`
- Additional Resources `#additional-resources`

---

## Overview of FRAME
https://docs.polkadot.com/parachains/customize-runtime.md

Learn how Polkadot SDK’s FRAME framework simplifies blockchain development with modular pallets and support libraries for efficient runtime design.

### Sections
- Introduction `#introduction`
- Understanding Your Runtime `#understanding-your-runtime`
- Runtime Architecture `#runtime-architecture`
- Building Blocks: Pallets `#building-blocks-pallets`
- Pre-Built Pallets vs. Custom Pallets `#pre-built-pallets-vs-custom-pallets`
- Pallet Structure `#pallet-structure`
- How Runtime Customization Works `#how-runtime-customization-works`
- Starting Templates `#starting-templates`
- Key Customization Scenarios `#key-customization-scenarios`

---

## Overview of the Polkadot Relay Chain
https://docs.polkadot.com/reference/polkadot-hub/consensus-and-security/relay-chain.md

Explore Polkadot's core architecture, including its multi-chain vision, shared security, and the DOT token's governance and staking roles.

### Sections
- Introduction `#introduction`
- Polkadot 1.0 `#polkadot-10`
- High-Level Architecture `#high-level-architecture`
- Polkadot's Additional Functionalities `#polkadots-additional-functionalities`
- Polkadot's Resilience `#polkadots-resilience`
- Polkadot's Blockspace `#polkadots-blockspace`
- DOT Token `#dot-token`
- Redenomination of DOT `#redenomination-of-dot`
- The Planck Unit `#the-planck-unit`
- Uses for DOT `#uses-for-dot`
- JAM and the Road Ahead `#jam-and-the-road-ahead`

---

## Parachains Overview
https://docs.polkadot.com/reference/parachains.md

Learn about parachains, specialized blockchains on Polkadot that gain shared security and interoperability. Discover how they work and the tools to build them.

### Sections
- Introduction `#introduction`
- Polkadot SDK: Parachain Architecture `#polkadot-sdk-parachain-architecture`
- Substrate: The Foundation `#substrate-the-foundation`
- FRAME: Building Blocks for Your Runtime `#frame-building-blocks-for-your-runtime`
- Cumulus: Parachain-Specific Functionality `#cumulus-parachain-specific-functionality`
- Where to Go Next `#where-to-go-next`

---

## Polkadot Hub Overview
https://docs.polkadot.com/reference/polkadot-hub.md

Learn how Polkadot Hub serves as the entry point to Polkadot, providing access to smart contracts, staking, governance, identity management, and cross-ecosystem interoperability.

### Sections
- Introduction `#introduction`
- Polkadot Hub Capabilities `#polkadot-hub-capabilities`
- Core Components `#core-components`
- Smart Contracts `#smart-contracts`
- Asset Management `#asset-management`
- People Chain `#people-chain`
- Bridge Hub `#bridge-hub`
- Consensus & Security `#consensus-security`
- Collectives & DAOs `#collectives-daos`
- Where to Go Next `#where-to-go-next`

---

## Polkadot SDK Accounts
https://docs.polkadot.com/reference/parachains/accounts.md

Learn about account structures, balances, and address formats in the Polkadot SDK, including how to manage lifecycle, references, and balances.

### Sections
- Introduction `#introduction`
- Account Data Structure `#account-data-structure`
- Account `#account`
- Account Info `#account-info`
- Account Reference Counters `#account-reference-counters`
- Account Balance Types `#account-balance-types`
- Balance Types `#balance-types`
- Locks `#locks`
- Balance Types on Polkadot.js `#balance-types-on-polkadotjs`
- Address Formats `#address-formats`
- Basic Format `#basic-format`
- Address Type `#address-type`
- Address Length `#address-length`
- Checksum Types `#checksum-types`
- Validating Addresses `#validating-addresses`

---

## Randomness
https://docs.polkadot.com/reference/parachains/randomness.md

Explore the importance of randomness in PoS blockchains, focusing on Polkadot’s VRF-based approach to ensure fairness and security in validator selection.

### Sections
- Introduction `#introduction`
- VRF `#vrf`
- How VRF Works `#how-vrf-works`
- RANDAO `#randao`
- VDFs `#vdfs`
- Additional Resources `#additional-resources`

---

## Register a Local Asset
https://docs.polkadot.com/chain-interactions/token-operations/register-local-asset.md

Learn how to register a local asset on Polkadot Hub, including prerequisites, deposits, and step-by-step instructions using Polkadot.js Apps.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Register a Local Asset `#register-a-local-asset`
- Verify Asset Registration `#verify-asset-registration`
- Test Setup Environment `#test-setup-environment`

---

## Set Up the Polkadot SDK Parachain Template
https://docs.polkadot.com/parachains/launch-a-parachain/set-up-the-parachain-template.md

Learn how to set up and run the Polkadot SDK Parachain Template locally, creating a ready-to-customize foundation for your parachain.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Polkadot SDK Utility Tools `#polkadot-sdk-utility-tools`
- Clone the Template `#clone-the-template`
- Explore the Project Structure `#explore-the-project-structure`
- Compile the Runtime `#compile-the-runtime`
- Verify the Build `#verify-the-build`
- Run the Node Locally `#run-the-node-locally`
- Interact with the Node `#interact-with-the-node`
- Stop the Node `#stop-the-node`
- Where to Go Next `#where-to-go-next`

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

## Support
https://docs.polkadot.com/get-support.md

Start here to get developer support for Polkadot. Connect with the team, find help, and explore resources beyond the documentation.

### Sections
- Support Channels `#support-channels`
- Community Resources `#community-resources`
- AI Resources `#ai-resources`

---

## Technical Reference Overview
https://docs.polkadot.com/reference.md

Learn about Polkadot's technical architecture, governance framework, parachain ecosystem, and the tools you need to build and interact with the network.

### Sections
- Introduction `#introduction`
- Polkadot Hub `#polkadot-hub`
- Parachains `#parachains`
- On-Chain Governance `#on-chain-governance`
- Glossary `#glossary`
- Tools `#tools`
- Where to Go Next `#where-to-go-next`

---

## Transactions
https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/transactions.md

Learn how to construct, submit, and validate transactions in the Polkadot SDK, covering signed, unsigned, and inherent types of transactions.

### Sections
- Introduction `#introduction`
- What Is a Transaction? `#what-is-a-transaction`
- Signed Transactions `#signed-transactions`
- Unsigned Transactions `#unsigned-transactions`
- Inherent Transactions `#inherent-transactions`
- Transaction Formats `#transaction-formats`
- Types of Transaction Formats `#types-of-transaction-formats`
- Signed Transaction Data Structure `#signed-transaction-data-structure`
- Signed Extensions `#signed-extensions`
- Transaction Construction `#transaction-construction`
- Construct a Signed Transaction `#construct-a-signed-transaction`
- Transaction Encoding `#transaction-encoding`
- Customize Transaction Construction `#customize-transaction-construction`
- Lifecycle of a Transaction `#lifecycle-of-a-transaction`
- Define Transaction Properties `#define-transaction-properties`
- Process on a Block Authoring Node `#process-on-a-block-authoring-node`
- Validate and Queue `#validate-and-queue`
- Transaction Ordering and Priority `#transaction-ordering-and-priority`
- Transaction Execution `#transaction-execution`
- Transaction Mortality `#transaction-mortality`
- Unique Identifiers for Extrinsics `#unique-identifiers-for-extrinsics`
- Additional Resources `#additional-resources`

---

## Transactions Weights and Fees
https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/fees.md

Overview of transaction weights and fees in Polkadot SDK chains, detailing how fees are calculated using a defined formula and runtime specifics.

### Sections
- Introductions `#introductions`
- How Fees are Calculated `#how-fees-are-calculated`
- Using the Transaction Payment Pallet `#using-the-transaction-payment-pallet`
- Understanding the Inclusion Fee `#understanding-the-inclusion-fee`
- Accounts with an Insufficient Balance `#accounts-with-an-insufficient-balance`
- Fee Multipliers `#fee-multipliers`
- Transactions with Special Requirements `#transactions-with-special-requirements`
- Default Weight Annotations `#default-weight-annotations`
- Weights and Database Read/Write Operations `#weights-and-database-readwrite-operations`
- Dispatch Classes `#dispatch-classes`
- Dynamic Weights `#dynamic-weights`
- Post Dispatch Weight Correction `#post-dispatch-weight-correction`
- Custom Fees `#custom-fees`
- Custom Weights `#custom-weights`
- Additional Resources `#additional-resources`
