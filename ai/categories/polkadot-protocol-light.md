---
category: Polkadot Protocol
description: Protocol internals covering consensus, coretime, block production, cryptography, and chain architecture.
page_count: 33
token_estimate: 5189
updated: '2026-04-16T19:42:11.755721+00:00'
---

## Agile Coretime
https://docs.polkadot.com/reference/polkadot-hub/consensus-and-security/agile-coretime.md

Explore the efficient scheduling mechanisms to access Polkadot cores to produce blockspace continuously or on-demand.

### Sections
- Introduction `#introduction`
- Bulk Coretime `#bulk-coretime`
- Coretime Interlacing `#coretime-interlacing`
- Coretime Splitting `#coretime-splitting`
- On-Demand Coretime `#on-demand-coretime`

---

## Asynchronous Backing
https://docs.polkadot.com/reference/parachains/consensus/async-backing.md

Understand how asynchronous backing pipelines parachain block production, the protocol changes it introduces on the Relay Chain, and how parachains participate safely and efficiently.

### Sections
- Introduction `#introduction`
- Configurations `#configurations`
- Synchronous VS. Asynchronous Processing `#synchronous-vs-asynchronous-processing`

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

## Bridge Hub
https://docs.polkadot.com/reference/polkadot-hub/bridging.md

Learn about the Bridge Hub system parachain, a parachain that facilitates the interactions from Polkadot to the rest of Web3.

### Sections
- Introduction `#introduction`
- Trustless Bridging `#trustless-bridging`
- Bridging Components `#bridging-components`
- Ethereum-Specific Support `#ethereum-specific-support`
- Deployed Bridges `#deployed-bridges`
- Where to Go Next `#where-to-go-next`

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

## Collectives Chain
https://docs.polkadot.com/reference/polkadot-hub/collectives-and-daos.md

Learn how the Collectives chain provides infrastructure for governance organizations, enabling decentralized network stewardship and decision-making.

### Sections
- Introduction `#introduction`
- Key Collectives `#key-collectives`

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

## Data Storage
https://docs.polkadot.com/reference/polkadot-hub/data-storage.md

Technical reference for the Polkadot Bulletin Chain, a specialized data storage chain with IPFS-compatible content addressing for the Polkadot ecosystem.

### Sections
- Use Cases `#use-cases`
- Key Concepts `#key-concepts`
- Authorization `#authorization`
- Who Grants Authorization `#who-grants-authorization`
- Account Authorization `#account-authorization`
- Preimage Authorization `#preimage-authorization`
- Capacity Planning `#capacity-planning`
- Transaction Storage Pallet `#transaction-storage-pallet`
- IPFS Integration and Data Retrieval `#ipfs-integration-and-data-retrieval`
- How It Works `#how-it-works`
- Retrieval Methods `#retrieval-methods`
- Data Lifecycle `#data-lifecycle`
- Size Limits `#size-limits`
- Network Endpoints `#network-endpoints`
- Where to Go Next `#where-to-go-next`

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

## Elastic Scaling
https://docs.polkadot.com/reference/parachains/consensus/elastic-scaling.md

Learn how elastic scaling in Polkadot boosts parachain throughput, reduces latency, and supports dynamic, cost-efficient resource allocation.

### Sections
- Introduction `#introduction`
- How Elastic Scaling Works `#how-elastic-scaling-works`
- Benefits of Elastic Scaling `#benefits-of-elastic-scaling`
- Use Cases `#use-cases`
- Handling Sudden Traffic Spikes `#handling-sudden-traffic-spikes`
- Supporting Early-Stage Growth `#supporting-early-stage-growth`
- Scaling Massive IoT Networks `#scaling-massive-iot-networks`
- Powering Real-Time, Low-Latency Systems `#powering-real-time-low-latency-systems`

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

## Inclusion Pipeline
https://docs.polkadot.com/reference/parachains/consensus/inclusion-pipeline.md

Learn how Polkadot validates parachain blocks through the Inclusion Pipeline, detailing context, generation, backing, and inclusion stages.

### Sections
- Introduction `#introduction`
- Pipeline Stages `#pipeline-stages`
- Context `#context`
- Generation `#generation`
- Backing `#backing`
- Inclusion `#inclusion`

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

## Origins and Tracks
https://docs.polkadot.com/reference/governance/origins-tracks.md

Explore Polkadot's OpenGov origins and tracks system, defining privilege levels, decision processes, and tailored pathways for network proposals.

### Sections
- Introduction `#introduction`
- Origins `#origins`
- Tracks `#tracks`
- Additional Resources `#additional-resources`

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

## People Chain
https://docs.polkadot.com/reference/polkadot-hub/people-and-identity.md

Learn how People chain secures decentralized identity management, empowering users to control and verify digital identities without central authorities.

### Sections
- Introduction `#introduction`
- Identity Management System `#identity-management-system`
- Sub-Identities `#sub-identities`
- Verification Process `#verification-process`
- Judgment Requests `#judgment-requests`
- Judgment Classifications `#judgment-classifications`
- Registrars `#registrars`
- Where to Go Next `#where-to-go-next`

---

## Polkadot Hub Assets
https://docs.polkadot.com/reference/polkadot-hub/assets.md

Learn about asset management on Polkadot Hub, including on-chain assets, foreign asset integration, and XCM for cross-chain asset transfers.

### Sections
- Introduction `#introduction`
- Why Use Polkadot Hub? `#why-use-polkadot-hub`
- Types of Assets `#types-of-assets`
- Asset Structure `#asset-structure`
- Common Asset IDs `#common-asset-ids`
- How Native Assets Work `#how-native-assets-work`
- Asset Operations `#asset-operations`
- Roles and Permissions `#roles-and-permissions`
- Freezing Assets `#freezing-assets`
- Delegated Transfers `#delegated-transfers`
- How Foreign Assets Work `#how-foreign-assets-work`
- Moving Assets Across Chains `#moving-assets-across-chains`
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

## Polkadot Hub Smart Contracts
https://docs.polkadot.com/reference/polkadot-hub/smart-contracts.md

Learn how Polkadot Hub supports smart contracts through the REVM, a Rust-based Ethereum Virtual Machine compatible runtime.

### Sections
- Introduction `#introduction`
- REVM Smart Contracts `#revm-smart-contracts`
- PVM Smart Contracts `#pvm-smart-contracts`
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

## Proof of Stake Consensus
https://docs.polkadot.com/reference/polkadot-hub/consensus-and-security/pos-consensus.md

Explore Polkadot's consensus protocols for secure, scalable, and decentralized network operation, including NPoS, BABE, GRANDPA, and BEEFY.

### Sections
- Introduction `#introduction`
- Nominated Proof of Stake `#nominated-proof-of-stake`
- Hybrid Consensus `#hybrid-consensus`
- Block Production - BABE `#block-production-babe`
- Validator Participation `#validator-participation`
- Additional Resources `#additional-resources`
- Finality Gadget - GRANDPA `#finality-gadget-grandpa`
- Probabilistic vs. Provable Finality `#probabilistic-vs-provable-finality`
- Additional Resources `#additional-resources-2`
- Fork Choice `#fork-choice`
- Additional Resources `#additional-resources-3`
- Bridging - BEEFY `#bridging-beefy`

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

## Runtime API Calls
https://docs.polkadot.com/chain-interactions/query-data/runtime-api-calls.md

Execute Polkadot SDK runtime APIs — including AccountNonceApi, Metadata, TransactionPaymentApi, and StakingApi — against Polkadot Hub TestNet using PAPI (`polkadot-api`), Polkadot.js API (`@polkadot/api`, maintenance mode), Dedot, Python Substrate Interface, or Subxt (Rust). Unlike storage queries, runtime APIs invoke Wasm runtime logic to return computed results. Requires Node.js 18+, Python 3.8+, or Rust stable toolchain depending on chosen SDK.

### Sections
- Call Runtime APIs `#call-runtime-apis`
- Available Runtime APIs `#available-runtime-apis`

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
