---
category: Chain Interactions
description: How to query on-chain data, send transactions, communicate across chains, and manage tokens and accounts on Polkadot-based chains.
page_count: 12
token_estimate: 1877
updated: '2026-04-16T19:42:11.755721+00:00'
---

## Calculate Transaction Fees
https://docs.polkadot.com/chain-interactions/send-transactions/calculate-transaction-fees.md

Learn how to calculate transaction fees on Polkadot using Polkadot-API, Polkadot.js API, and the Polkadot.js Apps UI to estimate transfer costs.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Polkadot-API (PAPI) `#polkadot-api-papi`
- Polkadot.js API `#polkadotjs-api`
- Polkadot.js Apps Interface `#polkadotjs-apps-interface`
- Estimate Fees via Transfer Interface `#estimate-fees-via-transfer-interface`
- Where to Go Next `#where-to-go-next`

---

## Chain Interactions Overview
https://docs.polkadot.com/chain-interactions.md

Learn how to query data, send transactions, enable cross-chain communication, and manage accounts across the Polkadot ecosystem.

### Sections
- Introduction `#introduction`
- Core Interaction Patterns `#core-interaction-patterns`
- Query On-Chain Data `#query-on-chain-data`
- Send Transactions `#send-transactions`
- Send Cross-Chain Transactions `#send-cross-chain-transactions`
- Manage Tokens `#manage-tokens`
- Manage Accounts `#manage-accounts`
- Development Tools and SDKs `#development-tools-and-sdks`
- Next Steps `#next-steps`

---

## Convert Assets on Asset Hub
https://docs.polkadot.com/chain-interactions/token-operations/convert-assets.md

A guide detailing the step-by-step process of converting assets on Asset Hub, helping users efficiently navigate asset management on the platform.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Create a Liquidity Pool `#create-a-liquidity-pool`
- Add Liquidity to a Pool `#add-liquidity-to-a-pool`
- Swap Assets `#swap-assets`
- Swap from an Exact Amount of Tokens `#swap-from-an-exact-amount-of-tokens`
- Swap to an Exact Amount of Tokens `#swap-to-an-exact-amount-of-tokens`
- Withdraw Liquidity from a Pool `#withdraw-liquidity-from-a-pool`
- Test Environment Setup `#test-environment-setup`

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

## Pay Transaction Fees with Different Tokens
https://docs.polkadot.com/chain-interactions/send-transactions/pay-fees-with-different-tokens.md

Learn how to send a DOT transfer transaction while paying the fees using a different token on Polkadot Hub using multiple SDKs.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Local Polkadot Hub Setup `#local-polkadot-hub-setup`
- Set Up Your Project `#set-up-your-project`
- Implementation `#implementation`
- Import Dependencies and Define Constants `#import-dependencies-and-define-constants`
- Create a Signer and Connect `#create-a-signer-and-connect`
- Create the Transaction `#create-the-transaction`
- Sign and Submit with Alternative Fee Payment `#sign-and-submit-with-alternative-fee-payment`
- Full Code `#full-code`
- Run the Script `#run-the-script`
- Expected Output `#expected-output`
- Conclusion `#conclusion`
- Where to Go Next `#where-to-go-next`

---

## Query Account Information with SDKs
https://docs.polkadot.com/chain-interactions/accounts/query-accounts.md

Learn how to query account information using five popular SDKs—Polkadot API (PAPI), Polkadot.js API, Dedot, Python Substrate Interface, and Subxt.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Query Account Information `#query-account-information`
- Understanding Account Data `#understanding-account-data`
- Where to Go Next `#where-to-go-next`

---

## Query On-Chain State with SDKs
https://docs.polkadot.com/chain-interactions/query-data/query-sdks.md

Query on-chain storage (System.Account balances, Assets pallet metadata) on Polkadot Hub using PAPI (`polkadot-api`), Polkadot.js API (`@polkadot/api`, maintenance mode — prefer PAPI or Dedot for new projects), Dedot, Python Substrate Interface (`substrate-interface`), or Subxt (Rust). Covers both account balance and asset information queries with runnable code examples. Requires Node.js 18+, Python 3.8+, or Rust stable toolchain depending on SDK.

### Sections
- Query On-Chain Data `#query-on-chain-data`

---

## Query On-Chain State with Sidecar REST API
https://docs.polkadot.com/chain-interactions/query-data/query-rest.md

Substrate API Sidecar exposes a REST HTTP interface for querying Polkadot SDK chain state without WebSocket connections or SDK setup. Covers querying native account balances, Assets pallet balances, asset metadata and details, foreign asset balances, and block data using `curl`. Public endpoints available for Polkadot, Kusama, Polkadot Hub, and Kusama Hub; can also be self-hosted via npm or Docker.

### Sections
- Prerequisites `#prerequisites`
- Running Sidecar Locally `#running-sidecar-locally`
- Public Sidecar Endpoints `#public-sidecar-endpoints`
- Query Account Balance `#query-account-balance`
- Query Asset Balances `#query-asset-balances`
- Query Asset Metadata `#query-asset-metadata`
- Query Asset Details `#query-asset-details`
- Query Foreign Asset Balances `#query-foreign-asset-balances`
- Query Block Information `#query-block-information`
- API Reference `#api-reference`

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

## Send Transactions with SDKs
https://docs.polkadot.com/chain-interactions/send-transactions/with-sdks.md

Learn how to construct, sign, and submit transactions using PAPI, Polkadot.js, Dedot, Python Substrate Interface, and Subxt.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Send Transactions `#send-transactions`
- Where to Go Next `#where-to-go-next`

---

## Store and Retrieve Data on the Bulletin Chain
https://docs.polkadot.com/chain-interactions/store-data/bulletin-chain.md

Learn how to store and retrieve an image on the Polkadot Bulletin Chain using the Console UI or PAPI, with step-by-step instructions.

### Sections
- Prerequisites `#prerequisites`
- Get Authorization `#get-authorization`
- Store Your Image `#store-your-image`
- Verify Your Stored Image `#verify-your-stored-image`
- Retrieve Your Data `#retrieve-your-data`
- Renew Your Data `#renew-your-data`
- Where to Go Next `#where-to-go-next`

---

## XCM Fee Estimation
https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/estimate-xcm-fees.md

This tutorial demonstrates how to estimate the fees for teleporting assets from the Polkadot Hub TestNet to the Paseo People Chain.

### Sections
- Introduction `#introduction`
- Fee Mechanism `#fee-mechanism`
- Environment Setup `#environment-setup`
- Client and API Setup `#client-and-api-setup`
- Create the XCM Message `#create-the-xcm-message`
- Fee Estimation Function `#fee-estimation-function`
- Complete Implementation `#complete-implementation`
- Full Code `#full-code`
- Running the Script `#running-the-script`
- Conclusion `#conclusion`
