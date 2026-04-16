---
category: Infrastructure
description: Running validators, collators, and RPC nodes, plus staking mechanics and key management.
page_count: 18
token_estimate: 3118
updated: '2026-04-16T19:42:11.755721+00:00'
---

## General Management
https://docs.polkadot.com/node-infrastructure/run-a-validator/operational-tasks/general-management.md

Optimize your Polkadot validator setup with advanced configuration techniques. Learn how to boost performance, enhance security, and ensure seamless operations.

### Sections
- Introduction `#introduction`
- Configuration Optimization `#configuration-optimization`
- Deactivate Simultaneous Multithreading `#deactivate-simultaneous-multithreading`
- Deactivate Automatic NUMA Balancing `#deactivate-automatic-numa-balancing`
- Spectre and Meltdown Mitigations `#spectre-and-meltdown-mitigations`
- Monitor Your Node `#monitor-your-node`
- Environment Setup `#environment-setup`
- Install and Configure Prometheus `#install-and-configure-prometheus`
- Start Prometheus `#start-prometheus`
- Install and Configure Grafana `#install-and-configure-grafana`
- Install and Configure Alertmanager `#install-and-configure-alertmanager`
- Secure Your Validator `#secure-your-validator`
- Key Management `#key-management`
- Signing Outside the Client `#signing-outside-the-client`
- Secure-Validator Mode `#secure-validator-mode`
- Linux Best Practices `#linux-best-practices`
- Validator Best Practices `#validator-best-practices`
- Additional Resources `#additional-resources`

---

## Node Infrastructure
https://docs.polkadot.com/node-infrastructure.md

From running RPC endpoints to producing parachain blocks or validating the relay chain, this guide explains your options and how to begin.

### Sections
- Introduction `#introduction`
- Node Types `#node-types`
- RPC Nodes `#rpc-nodes`
- Collators `#collators`
- Validators `#validators`
- Next Steps `#next-steps`

---

## Offenses and Slashes
https://docs.polkadot.com/node-infrastructure/run-a-validator/staking-mechanics/offenses-and-slashes.md

Learn about how Polkadot discourages validator misconduct via an offenses and slashing system, including details on offenses and their consequences.

### Sections
- Introduction `#introduction`
- Offenses `#offenses`
- Invalid Votes `#invalid-votes`
- Equivocations `#equivocations`
- Penalties `#penalties`
- Slashing `#slashing`
- Disabling `#disabling`
- Reputation Changes `#reputation-changes`
- Penalties by Offense `#penalties-by-offense`

---

## Pause Validating
https://docs.polkadot.com/node-infrastructure/run-a-validator/operational-tasks/pause-validating.md

Learn how to temporarily pause staking activity in Polkadot using the chill extrinsic, with guidance for validators and nominators.

### Sections
- Introduction `#introduction`
- Chilling Your Node `#chilling-your-node`
- Staking Election Timing Considerations `#staking-election-timing-considerations`
- Chilling as a Nominator `#chilling-as-a-nominator`
- Chilling as a Validator `#chilling-as-a-validator`
- Chill Other `#chill-other`

---

## Rewards Payout
https://docs.polkadot.com/node-infrastructure/run-a-validator/staking-mechanics/rewards.md

Learn how validator rewards work on the network, including era points, payout distribution, running multiple validators, and nominator payments.

### Sections
- Introduction `#introduction`
- Era Points `#era-points`
- Reward Variance `#reward-variance`
- Payout Scheme `#payout-scheme`
- Running Multiple Validators `#running-multiple-validators`
- Nominators and Validator Payments `#nominators-and-validator-payments`

---

## Run a Block-Producing Collator
https://docs.polkadot.com/node-infrastructure/run-a-collator.md

Learn how to set up and run a block-producing collator for Polkadot system parachains, including registration and session key management.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Hardware Requirements `#hardware-requirements`
- Software Requirements `#software-requirements`
- Account Requirements `#account-requirements`
- Install Dependencies `#install-dependencies`
- Generate Node Key `#generate-node-key`
- Obtain Chain Specification `#obtain-chain-specification`
- Run the Collator `#run-the-collator`
- Generate Session Keys `#generate-session-keys`
- Register Collator for Selection `#register-collator-for-selection`
- Registration Process `#registration-process`
- Commands for Node Management `#commands-for-node-management`
- Commands for Log Management `#commands-for-log-management`
- Database Maintenance `#database-maintenance`
- Updates and Upgrades `#updates-and-upgrades`
- Conclusion `#conclusion`

---

## Run a Parachain RPC Node
https://docs.polkadot.com/node-infrastructure/run-a-node/parachain-rpc.md

Follow this guide to understand hardware and software requirements and how to set up and run an RPC node for any parachain, including system parachains.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Hardware Requirements `#hardware-requirements`
- Software Requirements `#software-requirements`
- Obtain the Chain Specification `#obtain-the-chain-specification`
- System Parachains `#system-parachains`
- Other Parachains `#other-parachains`
- Spin Up a Node `#spin-up-a-node`
- Port Mappings `#port-mappings`
- Node Configuration Parameters `#node-configuration-parameters`
- Monitor Node Synchronization `#monitor-node-synchronization`
- Commands for Managing Your Node `#commands-for-managing-your-node`
- Conclusion `#conclusion`

---

## Run an RPC Node for Polkadot Hub
https://docs.polkadot.com/node-infrastructure/run-a-node/polkadot-hub-rpc.md

Learn how to set up and run an RPC node for Polkadot Hub with Polkadot SDK RPC endpoints and optional Ethereum JSON-RPC compatibility.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Hardware Requirements `#hardware-requirements`
- Software Requirements `#software-requirements`
- Spin Up a Node `#spin-up-a-node`
- Port Mappings `#port-mappings`
- Node Configuration Arguments `#node-configuration-arguments`
- Monitor Node Synchronization `#monitor-node-synchronization`
- Commands for Managing Your Node `#commands-for-managing-your-node`
- Ethereum RPC Compatibility `#ethereum-rpc-compatibility`
- Prerequisites `#prerequisites-2`
- Run the Ethereum RPC Adapter `#run-the-ethereum-rpc-adapter`
- Ethereum RPC Configuration `#ethereum-rpc-configuration`
- API Endpoints `#api-endpoints`
- Verify the Ethereum RPC Adapter `#verify-the-ethereum-rpc-adapter`
- Manage the Ethereum RPC Adapter `#manage-the-ethereum-rpc-adapter`
- Conclusion `#conclusion`

---

## Set Up a Bootnode
https://docs.polkadot.com/node-infrastructure/run-a-node/relay-chain/bootnode.md

Learn how to configure and run a bootnode for Polkadot, including P2P, WS, and secure WSS connections with network key management and proxies.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Accessing the Bootnode `#accessing-the-bootnode`
- Node Key `#node-key`
- Running the Bootnode `#running-the-bootnode`
- Testing Bootnode Connection `#testing-bootnode-connection`
- P2P `#p2p`
- P2P/WS `#p2pws`
- P2P/WSS `#p2pwss`

---

## Set Up a Node
https://docs.polkadot.com/node-infrastructure/run-a-node/relay-chain/full-node.md

Learn how to install, configure, and run Polkadot nodes, including setting up different node types and connecting to the network.

### Sections
- Introduction `#introduction`
- Set Up a Node `#set-up-a-node`
- Prerequisites `#prerequisites`
- Install and Build the Polkadot Binary `#install-and-build-the-polkadot-binary`
- Use Docker `#use-docker`
- Configure and Run Your Node `#configure-and-run-your-node`
- RPC Configurations `#rpc-configurations`
- Sync Your Node `#sync-your-node`
- Connect to Your Node `#connect-to-your-node`

---

## Set Up a Validator
https://docs.polkadot.com/node-infrastructure/run-a-validator/onboarding-and-offboarding/set-up-validator.md

Set up a Polkadot validator node to secure the network and earn staking rewards. Follow this step-by-step guide to install, configure, and manage your node.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Initial Setup `#initial-setup`
- Install Network Time Protocol Client `#install-network-time-protocol-client`
- Verify Landlock is Activated `#verify-landlock-is-activated`
- Install the Polkadot Binaries `#install-the-polkadot-binaries`
- Install from Official Releases `#install-from-official-releases`
- Install with Package Managers `#install-with-package-managers`
- Install with Ansible `#install-with-ansible`
- Install with Docker `#install-with-docker`
- Build from Sources `#build-from-sources`
- Verify Installation `#verify-installation`

---

## Set Up Secure WebSocket
https://docs.polkadot.com/node-infrastructure/run-a-node/relay-chain/secure-wss.md

Instructions on enabling SSL for your node and setting up a secure WebSocket proxy server using nginx for remote connections.

### Sections
- Introduction `#introduction`
- Secure a WebSocket Port `#secure-a-websocket-port`
- Obtain an SSL Certificate `#obtain-an-ssl-certificate`
- Install a Proxy Server `#install-a-proxy-server`
- Use nginx `#use-nginx`
- Use Apache2 `#use-apache2`
- Connect to the Node `#connect-to-the-node`

---

## Staking Operator Proxy on Polkadot
https://docs.polkadot.com/node-infrastructure/run-a-validator/operational-tasks/staking-operator-proxy.md

Learn how the Staking Operator proxy enables non-custodial validator operations by separating fund management from node operations on Polkadot.

### Sections
- Introduction `#introduction`
- Staking Operator vs Staking Proxy `#staking-operator-vs-staking-proxy`
- Set Up a Staking Operator Proxy `#set-up-a-staking-operator-proxy`
- Operate a Validator with a Staking Operator Proxy `#operate-a-validator-with-a-staking-operator-proxy`
- Manage Session Keys on Polkadot Hub `#manage-session-keys-on-polkadot-hub`
- Security Considerations `#security-considerations`

---

## Start Validating
https://docs.polkadot.com/node-infrastructure/run-a-validator/onboarding-and-offboarding/start-validating.md

Learn how to start validating on Polkadot by choosing a network, syncing your node, bonding DOT tokens, and activating your validator.

### Sections
- Introduction `#introduction`
- Choose a Network `#choose-a-network`
- Synchronize Chain Data `#synchronize-chain-data`
- Database Snapshot Services `#database-snapshot-services`
- Bond DOT `#bond-dot`
- Bonding DOT on Polkadot.js Apps `#bonding-dot-on-polkadotjs-apps`
- Validate `#validate`
- Verify Sync via Telemetry `#verify-sync-via-telemetry`
- Activate using Polkadot.js Apps `#activate-using-polkadotjs-apps`
- Monitor Validation Status and Slots `#monitor-validation-status-and-slots`
- Run a Validator Using Systemd `#run-a-validator-using-systemd`
- Create the Systemd Service File `#create-the-systemd-service-file`
- Run the Service `#run-the-service`

---

## Stop Validating
https://docs.polkadot.com/node-infrastructure/run-a-validator/onboarding-and-offboarding/stop-validating.md

Learn to safely stop validating on Polkadot, including chilling, unbonding tokens, and purging validator keys.

### Sections
- Introduction `#introduction`
- Pause Versus Stop `#pause-versus-stop`
- Chill Validator `#chill-validator`
- Purge Validator Session Keys `#purge-validator-session-keys`
- Unbond Your Tokens `#unbond-your-tokens`

---

## Upgrade a Validator Node
https://docs.polkadot.com/node-infrastructure/run-a-validator/operational-tasks/upgrade-your-node.md

Guide to seamlessly upgrading your Polkadot validator node, managing session keys, and executing server maintenance while avoiding downtime and slashing risks.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Session Keys `#session-keys`
- Keystore `#keystore`
- Upgrade Using Backup Validator `#upgrade-using-backup-validator`
- Session `N` `#session-n`
- Session `N+3` `#session-n3`

---

## Validator Key Management
https://docs.polkadot.com/node-infrastructure/run-a-validator/onboarding-and-offboarding/key-management.md

Learn how to generate and manage validator keys, including session keys for consensus participation and node keys for maintaining a stable network identity.

### Sections
- Introduction `#introduction`
- Set Session Keys `#set-session-keys`
- Generate Session Keys `#generate-session-keys`
- Submit Transaction to Set Keys `#submit-transaction-to-set-keys`
- Verify Session Key Setup `#verify-session-key-setup`
- Set the Node Key `#set-the-node-key`
- Generate the Node Key `#generate-the-node-key`
- Set Node Key `#set-node-key`

---

## Validator Requirements
https://docs.polkadot.com/node-infrastructure/run-a-validator/requirements.md

Explore the technical and system requirements for running a Polkadot validator, including setup, hardware, staking prerequisites, and security best practices.

### Sections
- Introduction `#introduction`
- Prerequisites `#prerequisites`
- Minimum Hardware Requirements `#minimum-hardware-requirements`
- VPS Provider List `#vps-provider-list`
- Minimum Bond Requirement `#minimum-bond-requirement`
- Minimum Validator Self-Stake `#minimum-validator-self-stake`
- Minimum Commission `#minimum-commission`
