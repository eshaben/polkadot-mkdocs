---
category: Interoperability
description: Cross-chain communication via XCM, asset transfers, HRMP channels, and bridging to external networks.
page_count: 12
token_estimate: 1984
updated: '2026-04-16T19:42:11.755721+00:00'
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

## ParaSpell XCM SDK
https://docs.polkadot.com/reference/tools/paraspell.md

A powerful open-source library that simplifies XCM integration, enabling developers to easily build interoperable dApps on Polkadot.

### Sections
- Introduction `#introduction`
- ParaSpell XCM SDK `#paraspell-xcm-sdk`
- Install ParaSpell `#install-paraspell`
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

## Replay and Dry Run XCMs
https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/debug-and-preview-xcms.md

Replay and dry-run XCMs using Chopsticks with full logging enabled. Diagnose issues, trace message flow, and debug complex cross-chain interactions.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Set Up Your Project `#set-up-your-project`
- Capture the XCM to Replay `#capture-the-xcm-to-replay`
- Fork the Relevant Chains `#fork-the-relevant-chains`
- Set the Block Numbers `#set-the-block-numbers`
- Enable Logging and Wasm Override `#enable-logging-and-wasm-override`
- Identify and Extract the XCM `#identify-and-extract-the-xcm`
- Replay the XCM `#replay-the-xcm`
- Add the Polkadot Hub Descriptor `#add-the-polkadot-hub-descriptor`
- Create a Replay Script `#create-a-replay-script`
- Execute the Replay Script `#execute-the-replay-script`
- Expected Output `#expected-output`
- Dry Run the XCM `#dry-run-the-xcm`
- Create a Dry Run Script `#create-a-dry-run-script`
- Execute the Dry Run Script `#execute-the-dry-run-script`
- Review and Debug `#review-and-debug`
- Where to Go Next `#where-to-go-next`

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

## Transfer Assets from Ethereum into Polkadot
https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-into-polkadot.md

A step-by-step guide to bridging assets from Ethereum to Polkadot using the ParaSpell XCM SDK and Snowbridge.

### Sections
- Introduction `#introduction`
- How the Bridge Works `#how-the-bridge-works`
- Prerequisites `#prerequisites`
- Prepare Tokens for Bridging `#prepare-tokens-for-bridging`
- Initialize Your Project `#initialize-your-project`
- Approve Tokens for Bridging `#approve-tokens-for-bridging`
- Build and Execute the Bridge Transfer `#build-and-execute-the-bridge-transfer`
- Monitor the Transfer `#monitor-the-transfer`
- Supported Assets and Destinations `#supported-assets-and-destinations`
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

---

## XCM Tools
https://docs.polkadot.com/reference/tools/xcm-tools.md

Explore essential XCM tools across Polkadot, crafted to enhance cross-chain functionality and integration within the ecosystem.

### Sections
- Introduction `#introduction`
- Popular XCM Tools `#popular-xcm-tools`
- Moonsong Labs XCM Tools `#moonsong-labs-xcm-tools`
- ParaSpell `#paraspell`
- Astar XCM Tools `#astar-xcm-tools`
- Chopsticks `#chopsticks`
- Moonbeam XCM SDK `#moonbeam-xcm-sdk`
