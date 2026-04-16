---
category: Parachains
description: Building and deploying parachains with the Polkadot SDK, including runtime development, pallets, and coretime.
page_count: 35
token_estimate: 6590
updated: '2026-04-16T19:42:11.755721+00:00'
---

## Add an Existing Pallet to the Runtime
https://docs.polkadot.com/parachains/customize-runtime/add-existing-pallets.md

Learn how to include and configure pallets in a Polkadot SDK-based runtime, from adding dependencies to implementing necessary traits.

### Sections
- Introduction `#introduction`
- Check Prerequisites `#check-prerequisites`
- Add an Existing Polkadot SDK Pallet to Your Runtime `#add-an-existing-polkadot-sdk-pallet-to-your-runtime`
- Add an Existing Pallet as a Dependency `#add-an-existing-pallet-as-a-dependency`
- Enable Standard Library Features `#enable-standard-library-features`
- Review the Config Trait `#review-the-config-trait`
- Implement the Config Trait `#implement-the-config-trait`
- Add to Runtime Construct `#add-to-runtime-construct`
- Verify the Runtime Compiles `#verify-the-runtime-compiles`
- Run Your Chain Locally `#run-your-chain-locally`
- Generate a Chain Specification `#generate-a-chain-specification`
- Start the Parachain Node `#start-the-parachain-node`
- Interact with the Pallet `#interact-with-the-pallet`
- Where to Go Next `#where-to-go-next`

---

## Add Multiple Pallet Instances
https://docs.polkadot.com/parachains/customize-runtime/add-pallet-instances.md

Learn how to implement multiple instances of the same pallet in your Polkadot SDK-based runtime, from adding dependencies to configuring unique instances.

### Sections
- Introduction `#introduction`
- Check Prerequisites `#check-prerequisites`
- Understanding Instantiable Pallets `#understanding-instantiable-pallets`
- Identifying an Instantiable Pallet `#identifying-an-instantiable-pallet`
- How Instance Generics Work `#how-instance-generics-work`
- Add Multiple Instances of a Pallet to Your Runtime `#add-multiple-instances-of-a-pallet-to-your-runtime`
- Add the Pallet as a Dependency `#add-the-pallet-as-a-dependency`
- Enable Standard Library Features `#enable-standard-library-features`
- Review the Config Trait `#review-the-config-trait`
- Define Pallet Parameters `#define-pallet-parameters`
- Import Instance Types `#import-instance-types`
- Implement Config Trait for First Instance `#implement-config-trait-for-first-instance`
- Implement Config Trait for Second Instance `#implement-config-trait-for-second-instance`
- Add Instances to Runtime Construct `#add-instances-to-runtime-construct`
- Verify the Runtime Compiles `#verify-the-runtime-compiles`
- Run Your Chain Locally `#run-your-chain-locally`
- Generate a Chain Specification `#generate-a-chain-specification`
- Start the Parachain Node `#start-the-parachain-node`
- Interact with Both Pallet Instances `#interact-with-both-pallet-instances`
- Test Instance Independence `#test-instance-independence`
- Where to Go Next `#where-to-go-next`

---

## Add Smart Contract Functionality
https://docs.polkadot.com/parachains/customize-runtime/add-smart-contract-functionality.md

Add smart contract capabilities to your Polkadot SDK-based blockchain. Explore PVM, EVM, and Wasm integration for enhanced chain functionality.

### Sections
- Introduction `#introduction`
- pallet-revive `#pallet-revive`
- Core Components `#core-components`
- Supported Languages and Compilers `#supported-languages-and-compilers`
- How It Works `#how-it-works`
- Key Benefits `#key-benefits`
- Implementation Examples `#implementation-examples`
- Frontier `#frontier`
- Integration Options `#integration-options`
- EVM Execution Only `#evm-execution-only`
- Full Ethereum Compatibility `#full-ethereum-compatibility`
- Key Benefits `#key-benefits-2`
- Implementation Examples `#implementation-examples-2`
- pallet-contracts (Legacy) `#pallet-contracts-legacy`
- Implementation Example `#implementation-example`
- Where to Go Next `#where-to-go-next`

---

## Benchmark Your Pallet
https://docs.polkadot.com/parachains/customize-runtime/pallet-development/benchmark-pallet.md

Learn how to benchmark extrinsics in your custom pallet to generate precise weight calculations suitable for production use.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Create the Benchmarking Module `#create-the-benchmarking-module`
- Define the Weight Trait `#define-the-weight-trait`
- Add WeightInfo to Config `#add-weightinfo-to-config`
- Update Extrinsic Weight Annotations `#update-extrinsic-weight-annotations`
- Include the Benchmarking Module `#include-the-benchmarking-module`
- Configure Pallet Dependencies `#configure-pallet-dependencies`
- Update Mock Runtime `#update-mock-runtime`
- Configure Runtime Benchmarking `#configure-runtime-benchmarking`
- Test Benchmark Compilation `#test-benchmark-compilation`
- Build the Runtime with Benchmarks `#build-the-runtime-with-benchmarks`
- Install the Benchmarking Tool `#install-the-benchmarking-tool`
- Download the Weight Template `#download-the-weight-template`
- Execute Benchmarks `#execute-benchmarks`
- Use Generated Weights `#use-generated-weights`
- Related Resources `#related-resources`

---

## Chopsticks
https://docs.polkadot.com/reference/tools/chopsticks.md

Chopsticks is a versatile tool for forking live Polkadot SDK chains, enabling local testing, block replay, and XCM simulation without deploying to live networks.

### Sections
- Introduction `#introduction`
- Key Features `#key-features`
- Prerequisites `#prerequisites`
- Installation `#installation`
- Global Installation `#global-installation`
- Local Installation `#local-installation`
- Get Started `#get-started`
- Configuration Options `#configuration-options`
- Configuration File `#configuration-file`
- Create a Fork `#create-a-fork`
- Interact with a Fork `#interact-with-a-fork`
- Replay Blocks `#replay-blocks`
- Test XCM `#test-xcm`
- WebSocket Commands `#websocket-commands`
- Where to Go Next `#where-to-go-next`

---

## Coretime Renewal
https://docs.polkadot.com/parachains/runtime-maintenance/coretime-renewal.md

Learn how to renew coretime manually or automatically to ensure uninterrupted parachain operation with predictable pricing and minimal risk.

### Sections
- Introduction `#introduction`
- Bulk Sale Phases `#bulk-sale-phases`
- Renewal Timing `#renewal-timing`
- Manual Renewal `#manual-renewal`
- Auto-Renewal `#auto-renewal`
- Set Up an HRMP Channel `#set-up-an-hrmp-channel`
- Fund Sovereign Account `#fund-sovereign-account`
- Auto-Renewal Configuration Extrinsics `#auto-renewal-configuration-extrinsics`
- Construct the Enable Auto-Renewal Extrinsic `#construct-the-enable-auto-renewal-extrinsic`
- Submit the XCM from Your Parachain `#submit-the-xcm-from-your-parachain`
- Where to Go Next `#where-to-go-next`

---

## Create a Custom Pallet
https://docs.polkadot.com/parachains/customize-runtime/pallet-development/create-a-pallet.md

Learn how to create custom pallets using FRAME, allowing for flexible, modular, and scalable blockchain development. Follow the step-by-step guide.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Core Pallet Components `#core-pallet-components`
- Create the Pallet Project `#create-the-pallet-project`
- Configure Dependencies `#configure-dependencies`
- Initialize the Pallet Structure `#initialize-the-pallet-structure`
- Configure the Pallet `#configure-the-pallet`
- Define Events `#define-events`
- Define Errors `#define-errors`
- Add Storage Items `#add-storage-items`
- Configure Genesis State `#configure-genesis-state`
- Implement Dispatchable Functions `#implement-dispatchable-functions`
- Dispatchable Function Details `#dispatchable-function-details`
- Verify Pallet Compilation `#verify-pallet-compilation`
- Add the Pallet to Your Runtime `#add-the-pallet-to-your-runtime`
- Add Runtime Dependency `#add-runtime-dependency`
- Implement the Config Trait `#implement-the-config-trait`
- Add to Runtime Construct `#add-to-runtime-construct`
- Configure Genesis for Your Runtime `#configure-genesis-for-your-runtime`
- Verify Runtime Compilation `#verify-runtime-compilation`
- Run Your Chain Locally `#run-your-chain-locally`
- Generate a Chain Specification `#generate-a-chain-specification`
- Start the Parachain Node `#start-the-parachain-node`
- Interact with Your Pallet `#interact-with-your-pallet`
- Key Takeaways `#key-takeaways`
- Where to Go Next `#where-to-go-next`

---

## Deploy on Polkadot
https://docs.polkadot.com/parachains/launch-a-parachain/deploy-to-polkadot.md

This guide walks you through the journey of deploying your Polkadot SDK parachain on the Polkadot TestNet, detailing each step to a successful deployment.

### Sections
- Introduction `#introduction`
- Get Started with an Account and Tokens `#get-started-with-an-account-and-tokens`
- Reserve a Parachain Identifier `#reserve-a-parachain-identifier`
- Generate Custom Keys for Your Collators `#generate-custom-keys-for-your-collators`
- Generate the Chain Specification `#generate-the-chain-specification`
- Export Required Files `#export-required-files`
- Register a Parathread `#register-a-parathread`
- Start the Collator Node `#start-the-collator-node`
- Producing Blocks `#producing-blocks`

---

## E2E Testing with Moonwall
https://docs.polkadot.com/reference/tools/moonwall.md

Enhance blockchain end-to-end testing with Moonwall's standardized environment setup, comprehensive configuration management, and simple network interactions.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Install Moonwall `#install-moonwall`
- Global Installation `#global-installation`
- Local Installation `#local-installation`
- Initialize Moonwall `#initialize-moonwall`
- Writing Tests `#writing-tests`
- Running the Tests `#running-the-tests`
- Where to Go Next `#where-to-go-next`

---

## Fork a Parachain Using Chopsticks
https://docs.polkadot.com/parachains/testing/fork-a-parachain.md

Simplify Polkadot SDK development with Chopsticks. Learn essential features, how to install Chopsticks, and how to configure local blockchain forks.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Install Chopsticks `#install-chopsticks`
- Global Installation `#global-installation`
- Local Installation `#local-installation`
- Configure Chopsticks `#configure-chopsticks`
- Configuration File `#configuration-file`
- CLI Flags `#cli-flags`
- WebSocket Commands `#websocket-commands`
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

## Indexers
https://docs.polkadot.com/parachains/integrations/indexers.md

Discover blockchain indexers. Enhance data access, enable fast and complex queries, and optimize blockchain data for seamless app performance.

### Sections
- The Challenge of Blockchain Data Access `#the-challenge-of-blockchain-data-access`
- What is a Blockchain Indexer? `#what-is-a-blockchain-indexer`
- Indexer Implementations `#indexer-implementations`

---

## Light Clients
https://docs.polkadot.com/reference/tools/light-clients.md

Light clients enable secure and efficient blockchain interaction without running a full node. Learn everything you need to know about light clients on Polkadot.

### Sections
- Introduction `#introduction`
- Light Clients Workflow `#light-clients-workflow`
- JSON-RPC and Light Client Comparison `#json-rpc-and-light-client-comparison`
- Using Light Clients `#using-light-clients`
- PAPI Light Client Support `#papi-light-client-support`
- Substrate Connect - Browser Extension `#substrate-connect-browser-extension`
- Resources `#resources`

---

## Mock Your Runtime
https://docs.polkadot.com/parachains/customize-runtime/pallet-development/mock-runtime.md

Learn how to create a mock runtime environment for testing your custom pallets in isolation, enabling comprehensive unit testing before runtime integration.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Understand Mock Runtimes `#understand-mock-runtimes`
- Create the Mock Runtime Module `#create-the-mock-runtime-module`
- Set Up Basic Mock `#set-up-basic-mock`
- Implement Essential Configuration `#implement-essential-configuration`
- Implement Your Pallet's Configuration `#implement-your-pallets-configuration`
- Configure Genesis Storage `#configure-genesis-storage`
- Basic Test Environment `#basic-test-environment`
- Custom Genesis Configurations `#custom-genesis-configurations`
- Verify Mock Compilation `#verify-mock-compilation`
- Key Takeaways `#key-takeaways`
- Where to Go Next `#where-to-go-next`

---

## Obtain Coretime
https://docs.polkadot.com/parachains/launch-a-parachain/obtain-coretime.md

Learn how to obtain coretime for block production with this guide, covering both on-demand and bulk options for smooth operations.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Order On-Demand Coretime `#order-on-demand-coretime`
- On-Demand Extrinsics `#on-demand-extrinsics`
- Place an On-Demand Order `#place-an-on-demand-order`
- Purchase Bulk Coretime `#purchase-bulk-coretime`
- Connect Your Wallet to RegionX `#connect-your-wallet-to-regionx`
- Obtain Coretime Chain Funds `#obtain-coretime-chain-funds`
- Purchase a Core `#purchase-a-core`
- Verify Your Purchase `#verify-your-purchase`
- Assign Your Parachain to the Core `#assign-your-parachain-to-the-core`
- Next Steps `#next-steps`

---

## Opening HRMP Channels Between Parachains
https://docs.polkadot.com/parachains/interoperability/channels-between-parachains.md

Learn how to open HRMP channels between parachains on Polkadot. Discover the step-by-step process for establishing uni- and bidirectional communication.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Procedure to Initiate an HRMP Channel `#procedure-to-initiate-an-hrmp-channel`
- Fund Sender Sovereign Account `#fund-sender-sovereign-account`
- Create Channel Opening Extrinsic `#create-channel-opening-extrinsic`
- Craft and Submit the XCM Message from the Sender `#craft-and-submit-the-xcm-message-from-the-sender`
- Procedure to Accept an HRMP Channel `#procedure-to-accept-an-hrmp-channel`
- Fund Receiver Sovereign Account `#fund-receiver-sovereign-account`
- Create Channel Accepting Extrinsic `#create-channel-accepting-extrinsic`
- Craft and Submit the XCM Message from the Receiver `#craft-and-submit-the-xcm-message-from-the-receiver`

---

## Opening HRMP Channels with System Parachains
https://docs.polkadot.com/parachains/interoperability/channels-with-system-parachains.md

Learn how to open HRMP channels with Polkadot system parachains. Discover the process for establishing bi-directional communication using a single XCM message.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Procedure to Establish an HRMP Channel `#procedure-to-establish-an-hrmp-channel`
- Fund Parachain Sovereign Account `#fund-parachain-sovereign-account`
- Create Establish Channel with System Extrinsic `#create-establish-channel-with-system-extrinsic`
- Craft and Submit the XCM Message `#craft-and-submit-the-xcm-message`

---

## Oracles
https://docs.polkadot.com/parachains/integrations/oracles.md

Learn about blockchain oracles, the essential bridges connecting blockchains with real-world data for decentralized applications in the Polkadot ecosystem.

### Sections
- What is a Blockchain Oracle? `#what-is-a-blockchain-oracle`
- Oracle Implementations `#oracle-implementations`

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

## Parachain Consensus
https://docs.polkadot.com/reference/parachains/consensus.md

Understand how the blocks authored by parachain collators are secured by the relay chain validators and how the parachain transactions achieve finality.

### Sections
- Introduction `#introduction`
- The Role of Collators `#the-role-of-collators`
- Consensus and Validation `#consensus-and-validation`
- Path of a Parachain Block `#path-of-a-parachain-block`
- Where to Go Next `#where-to-go-next`

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

## Polkadot Omni Node
https://docs.polkadot.com/reference/tools/omninode.md

Run parachain nodes easily with the polkadot-omni-node, a white-labeled binary that can run parachain nodes using a single pre-built solution.

### Sections
- Introduction `#introduction`
- Install Polkadot Omni Node `#install-polkadot-omni-node`
- Obtain Chain Specifications `#obtain-chain-specifications`
- Run a Parachain Full Node `#run-a-parachain-full-node`
- Interact with the Node `#interact-with-the-node`
- Parachain Compatibility `#parachain-compatibility`
- Required Runtime APIs `#required-runtime-apis`
- Required Pallets `#required-pallets`

---

## Quickstart Parachain Development with Pop CLI
https://docs.polkadot.com/reference/tools/pop-cli.md

Quickly bootstrap parachain projects, scaffold templates, deploy local networks, and streamline development workflows using Pop CLI.

### Sections
- Introduction `#introduction`
- Install Pop CLI `#install-pop-cli`
- Set Up Your Development Environment `#set-up-your-development-environment`
- Initialize a Project `#initialize-a-project`
- Where to Go Next `#where-to-go-next`

---

## Register a Foreign Asset
https://docs.polkadot.com/chain-interactions/token-operations/register-foreign-asset.md

Learn step-by-step how to register a foreign asset on Polkadot Hub and the origin parachain to enable cross-chain token transfers.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Register a Foreign Asset `#register-a-foreign-asset`
- Polkadot Hub `#polkadot-hub`
- Source Parachain `#source-parachain`
- Asset Registration Verification `#asset-registration-verification`
- Test Environment Setup `#test-environment-setup`

---

## Run a Parachain Network
https://docs.polkadot.com/parachains/testing/run-a-parachain-network.md

Learn how to deploy a local parachain test network using Zombienet, including building a custom collator and verifying block production.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up the Parachain Template `#set-up-the-parachain-template`
- Download Relay Chain Binaries `#download-relay-chain-binaries`
- Generate a Paseo Local Chain Spec `#generate-a-paseo-local-chain-spec`
- Configure the Network `#configure-the-network`
- Spawn the Network `#spawn-the-network`
- Verify the Network `#verify-the-network`
- Where to Go Next `#where-to-go-next`

---

## Runtime Upgrades
https://docs.polkadot.com/parachains/runtime-maintenance/runtime-upgrades.md

Learn how to safely perform runtime upgrades for your Polkadot SDK-based blockchain, including step-by-step instructions.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Start Your Chain `#start-your-chain`
- Add a New Feature `#add-a-new-feature`
- Bump the Runtime Version `#bump-the-runtime-version`
- Build the New Runtime `#build-the-new-runtime`
- Submit the Runtime Upgrade `#submit-the-runtime-upgrade`
- Verify the Upgrade `#verify-the-upgrade`
- Check Runtime Version `#check-runtime-version`
- Test New Functionality `#test-new-functionality`
- Where to Go Next `#where-to-go-next`

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

## Storage Migrations
https://docs.polkadot.com/parachains/runtime-maintenance/storage-migrations.md

Ensure smooth runtime upgrades with storage migrations, update data formats, and prevent errors. Learn when and how to implement migrations efficiently.

### Sections
- Introduction `#introduction`
- Storage Migration Scenarios `#storage-migration-scenarios`
- Implement Storage Migrations `#implement-storage-migrations`
- Core Migration Function `#core-migration-function`
- Migration Testing Hooks `#migration-testing-hooks`
- Migration Structure `#migration-structure`
- Migration Organization `#migration-organization`
- Scheduling Migrations `#scheduling-migrations`
- Single-Block Migrations `#single-block-migrations`
- Multi Block Migrations `#multi-block-migrations`

---

## Transfer Assets Between Parachains
https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-parachains.md

A step-by-step guide to using the ParaSpell XCM SDK to build, verify, and execute a transfer from one Parachain to another.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Initialize Your Project `#initialize-your-project`
- Build a Token Transfer Transaction `#build-a-token-transfer-transaction`
- Perform a Dry Run `#perform-a-dry-run`
- Verify the Existential Deposit `#verify-the-existential-deposit`
- Get Transfer Info and Fee Estimates `#get-transfer-info-and-fee-estimates`
- Next Steps `#next-steps`

---

## Unit Test Pallets
https://docs.polkadot.com/parachains/customize-runtime/pallet-development/pallet-testing.md

Learn how to efficiently test pallets in the Polkadot SDK, ensuring the reliability and security of your pallets operations.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Understanding FRAME Testing Tools `#understanding-frame-testing-tools`
- Assertion Macros `#assertion-macros`
- System Pallet Test Helpers `#system-pallet-test-helpers`
- Origin Types `#origin-types`
- Create the Tests Module `#create-the-tests-module`
- Set Up the Test Module `#set-up-the-test-module`
- Write Your First Test `#write-your-first-test`
- Test Basic Increment `#test-basic-increment`
- Test Error Conditions `#test-error-conditions`
- Test Overflow Protection `#test-overflow-protection`
- Test Underflow Protection `#test-underflow-protection`
- Test Access Control `#test-access-control`
- Test Root-Only Access `#test-root-only-access`
- Test Event Emission `#test-event-emission`
- Test Event Data `#test-event-data`
- Test Genesis Configuration `#test-genesis-configuration`
- Test Genesis Setup `#test-genesis-setup`
- Run All Tests `#run-all-tests`
- Where to Go Next `#where-to-go-next`

---

## Unlock a Parachain
https://docs.polkadot.com/parachains/runtime-maintenance/unlock-parachains.md

Learn how to unlock your parachain. This step-by-step guide covers verifying lock status, preparing calls, and executing the unlock process.

### Sections
- Introduction `#introduction`
- Check If the Parachain Is Locked `#check-if-the-parachain-is-locked`
- How to Unlock a Parachain `#how-to-unlock-a-parachain`
- Prepare the Unlock Call `#prepare-the-unlock-call`
- Fund the Sovereign Account `#fund-the-sovereign-account`
- Craft and Submit the XCM `#craft-and-submit-the-xcm`

---

## Wallets
https://docs.polkadot.com/parachains/integrations/wallets.md

Explore blockchain wallets. Securely manage digital assets with hot wallets for online access or cold wallets for offline, enhanced security.

### Sections
- What is a Blockchain Wallet? `#what-is-a-blockchain-wallet`
- Hot Wallets `#hot-wallets`
- Cold Wallets `#cold-wallets`
- Wallet Tools `#wallet-tools`

---

## Zombienet
https://docs.polkadot.com/reference/tools/zombienet.md

Zombienet is a testing framework for Polkadot SDK-based blockchain networks, enabling developers to deploy and test ephemeral environments across various platforms.

### Sections
- Introduction `#introduction`
- Install Zombienet `#install-zombienet`
- Providers `#providers`
- Kubernetes `#kubernetes`
- Podman `#podman`
- Native `#native`
- Configure Zombienet `#configure-zombienet`
- Basic Configuration `#basic-configuration`
- CLI Commands `#cli-commands`
- Settings `#settings`
- Relay Chain Configuration `#relay-chain-configuration`
- Parachain Configuration `#parachain-configuration`
- XCM Configuration `#xcm-configuration`
- Spawn a Network `#spawn-a-network`
- Write Tests `#write-tests`
- Test File Structure `#test-file-structure`
- Run Tests `#run-tests`
- Common Assertions `#common-assertions`
- Configuration Reference `#configuration-reference`
- Where to Go Next `#where-to-go-next`
