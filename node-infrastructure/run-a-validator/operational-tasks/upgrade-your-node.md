---
title: Upgrade a Validator Node
description: Guide to seamlessly upgrading your Polkadot validator node, managing session keys, and executing server maintenance while avoiding downtime and slashing risks.
categories:
- Infrastructure
url: https://docs.polkadot.com/node-infrastructure/run-a-validator/operational-tasks/upgrade-your-node/
word_count: 923
token_estimate: 1322
version_hash: sha256:b98a0638d344791f5d6a57c43d6c04dee208f60f3cf0233f8f2a259b8fca645d
last_updated: '2026-04-15T03:51:44+00:00'
---

# Upgrade a Validator Node

## Introduction

Upgrading a Polkadot validator node is essential for staying current with network updates and maintaining optimal performance. This guide covers routine and extended maintenance scenarios, including software upgrades and major server changes. Following these steps, you can manage session keys and transition smoothly between servers without risking downtime, slashing, or network disruptions. The process requires strategic planning, especially if you need to perform long-lead maintenance, ensuring your validator remains active and compliant.

This guide will allow validators to seamlessly substitute an active validator server to allow for maintenance operations. The process can take several hours, so ensure you understand the instructions first and plan accordingly.

## Prerequisites

Before beginning the upgrade process for your validator node, ensure the following:

- You have a fully functional validator setup with all required binaries installed. See [Set Up a Validator](/polkadot-mkdocs/node-infrastructure/run-a-validator/onboarding-and-offboarding/set-up-validator/) and [Validator Requirements](/polkadot-mkdocs/node-infrastructure/run-a-validator/requirements/) for additional guidance.
- Your VPS infrastructure has enough capacity to run a secondary validator instance temporarily for the upgrade process.

## Session Keys

Session keys are used to sign validator operations and establish a connection between your validator node and your staking proxy account. These keys are stored in the client, and any change to them requires a waiting period. Specifically, if you modify your session keys, the change will take effect only after the current session is completed and two additional sessions have passed.

Remembering this delayed effect when planning upgrades is crucial to ensure that your validator continues to function correctly and avoids interruptions. To learn more about session keys and their importance, visit the [Keys section](https://wiki.polkadot.com/learn/learn-cryptography/#keys).

## Keystore

Your validator server's `keystore` folder holds the private keys needed for signing network-level transactions. It is important not to duplicate or transfer this folder between validator instances. Doing so could result in multiple validators signing with the duplicate keys, leading to severe consequences such as [equivocation slashing](/polkadot-mkdocs/node-infrastructure/run-a-validator/staking-mechanics/offenses-and-slashes/#equivocation-slash). Instead, always generate new session keys for each validator instance.

The default path to the `keystore` is as follows:

```bash
/home/polkadot/.local/share/polkadot/chains/<chain>/keystore
```

Taking care to manage your keys securely ensures that your validator operates safely and without the risk of slashing penalties.

## Upgrade Using Backup Validator

The following instructions outline how to temporarily switch between two validator nodes. The original active validator is referred to as Validator A and the backup node used for maintenance purposes as Validator B.

### Session `N`

1. **Start Validator B**: Launch a secondary node and wait until it is fully synced with the network. Once synced, start it with the `--validator` flag. This node will now act as Validator B.
2. **Generate session keys**: Create new session keys for Validator B. See the [Key Management](/polkadot-mkdocs/node-infrastructure/run-a-validator/onboarding-and-offboarding/key-management/) guide for the current procedure.
3. **Submit session keys**: Use your staking proxy account to submit the session keys on-chain. See the [Key Management](/polkadot-mkdocs/node-infrastructure/run-a-validator/onboarding-and-offboarding/key-management/#submit-transaction-to-set-keys) guide for submission paths.
4. **Record the session**: Make a note of the session in which you executed this extrinsic.
5. **Wait for session changes**: Allow the current session to end and then wait for two additional full sessions for the new keys to take effect.

!!! warning "Keep Validator A running"

      It is crucial to keep Validator A operational during this entire waiting period. Since session key changes do not take effect immediately, turning off Validator A too early may result in chilling or even slashing.

### Session `N+3`

At this stage, Validator B becomes your active validator. You can now safely perform any maintenance tasks on Validator A.

Complete the following steps when you are ready to bring Validator A back online:

1. **Start Validator A**: Launch Validator A, sync the blockchain database, and ensure it is running with the `--validator` flag.
2. **Generate new session keys for Validator A**: Create fresh session keys for Validator A. See the [Key Management](/polkadot-mkdocs/node-infrastructure/run-a-validator/onboarding-and-offboarding/key-management/) guide for the current procedure.
3. **Submit session keys**: Using your staking proxy account, submit the session keys on-chain. See the [Key Management](/polkadot-mkdocs/node-infrastructure/run-a-validator/onboarding-and-offboarding/key-management/#submit-transaction-to-set-keys) guide for submission paths.
4. **Record the session**: Again, make a note of the session in which you executed this extrinsic.

Keep Validator B active until the session during which you executed the `set-key` extrinsic completes plus two additional full sessions have passed. Once Validator A has successfully taken over, you can safely stop Validator B. This process helps ensure a smooth handoff between nodes and minimizes the risk of downtime or penalties. Verify the transition by checking for finalized blocks in the new session. The logs should indicate the successful change, similar to the example below:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>INSERT_COMMAND</span>
  <span data-ty>2019-10-28 21:44:13 Applying authority set change scheduled at block #450092</span>
  <span data-ty>2019-10-28 21:44:13 Applying GRANDPA set change to new set with 20 authorities</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
