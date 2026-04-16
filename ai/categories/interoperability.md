---
category: Interoperability
includes_base_categories: true
base_categories:
- basics
- reference
word_count: 63797
token_estimate: 111068
page_count: 44
build_timestamp: '2026-04-16T19:42:11.755721+00:00'
version_hash: sha256:74afd0612d94a6a0ea6c20e63a3a52e19989a0dc900ea3a307a338cf7f3ebc56
---

# Begin New Bundle: Interoperability
Includes shared base categories: basics, reference


---

Page Title: Blocks

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/blocks.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/blocks/
- Summary: Understand how blocks are produced, validated, and imported in Polkadot SDK-based blockchains, covering initialization, finalization, and authoring processes.
- Word Count: 890; Token Estimate: 1335
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:4fb6cc5ba09140a53c043322725a40d8591e2f38e0a2ab2357c4c8cef00114e5

# Blocks

## Introduction

In the Polkadot SDK, blocks are fundamental to the functioning of the blockchain, serving as containers for [transactions](/polkadot-mkdocs/reference/parachains/blocks-transactions-fees/transactions/) and changes to the chain's state. Blocks consist of headers and an array of transactions, ensuring the integrity and validity of operations on the network. This guide explores the essential components of a block, the process of block production, and how blocks are validated and imported across the network. By understanding these concepts, developers can better grasp how blockchains maintain security, consistency, and performance within the Polkadot ecosystem.

## What is a Block?

In the Polkadot SDK, a block is a fundamental unit that encapsulates both the header and an array of transactions. The block header includes critical metadata to ensure the integrity and sequence of the blockchain. Here's a breakdown of its components:

- **Block height**: Indicates the number of blocks created in the chain so far.
- **Parent hash**: The hash of the previous block, providing a link to maintain the blockchain's immutability.
- **Transaction root**: Cryptographic digest summarizing all transactions in the block.
- **State root**: A cryptographic digest representing the post-execution state.
- **Digest**: Additional information that can be attached to a block, such as consensus-related messages.

Each transaction is part of a series that is executed according to the runtime's rules. The transaction root is a cryptographic digest of this series, which prevents alterations and enables succinct verification by light clients. This verification process allows light clients to confirm whether a transaction exists in a block with only the block header, avoiding downloading the entire block.

## Block Production

When an authoring node is authorized to create a new block, it selects transactions from the transaction queue based on priority. This step, known as block production, relies heavily on the executive module to manage the initialization and finalization of blocks. The process is summarized as follows:

### Initialize Block

The block initialization process begins with a series of function calls that prepare the block for transaction execution:

1. **Call `on_initialize`**: The executive module calls the [`on_initialize`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.Hooks.html#method.on_initialize) hook from the system pallet and other runtime pallets to prepare for the block's transactions.
2. **Coordinate runtime calls**: Coordinates function calls in the order defined by the transaction queue.
3. **Verify information**: Once [`on_initialize`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.Hooks.html#method.on_initialize) functions are executed, the executive module checks the parent hash in the block header and the trie root to verify information is consistent.

### Finalize Block

Once transactions are processed, the block must be finalized before being broadcast to the network. The finalization steps are as follows:

1. **Call `on_finalize`**: The executive module calls the [`on_finalize`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.Hooks.html#method.on_finalize) hooks in each pallet to ensure any remaining state updates or checks are completed before the block is sealed and published.
2. **Verify information**: The block's digest and storage root in the header are checked against the initialized block to ensure consistency.
3. **Call `on_idle`**: The [`on_idle`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.Hooks.html#method.on_idle) hook is triggered to process any remaining tasks using the leftover weight from the block.

## Block Authoring and Import

Once the block is finalized, it is gossiped to other nodes in the network. Nodes follow this procedure:

1. **Receive transactions**: The authoring node collects transactions from the network.
2. **Validate**: Transactions are checked for validity.
3. **Queue**: Valid transactions are placed in the transaction pool for execution.
4. **Execute**: State changes are made as the transactions are executed.
5. **Publish**: The finalized block is broadcast to the network.

### Block Import Queue

After a block is published, other nodes on the network can import it into their chain state. The block import queue is part of the outer node in every Polkadot SDK-based node and ensures incoming blocks are valid before adding them to the node's state.

In most cases, you don't need to know details about how transactions are gossiped or how other nodes on the network import blocks. The following traits are relevant, however, if you plan to write any custom consensus logic or want a deeper dive into the block import queue:

- **[`ImportQueue`](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/import_queue/trait.ImportQueue.html)**: The trait that defines the block import queue.
- **[`Link`](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/import_queue/trait.Link.html)**: The trait that defines the link between the block import queue and the network.
- **[`BasicQueue`](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/import_queue/struct.BasicQueue.html)**: A basic implementation of the block import queue.
- **[`Verifier`](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/import_queue/trait.Verifier.html)**: The trait that defines the block verifier.
- **[`BlockImport`](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/block_import/trait.BlockImport.html)**: The trait that defines the block import process.

These traits govern how blocks are validated and imported across the network, ensuring consistency and security.

## Additional Resources

To learn more about the block structure in the Polkadot SDK runtime, see the [`Block` reference](https://paritytech.github.io/polkadot-sdk/master/sp_runtime/traits/trait.Block.html) entry in the Rust Docs.


---

Page Title: Bridge Hub

- Resolved Markdown: https://docs.polkadot.com/reference/polkadot-hub/bridging.md
- Canonical (HTML): https://docs.polkadot.com/reference/polkadot-hub/bridging/
- Summary: Learn about the Bridge Hub system parachain, a parachain that facilitates the interactions from Polkadot to the rest of Web3.
- Word Count: 750; Token Estimate: 1139
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:f8f7b65ef30b209a7935e3d3fe5cdb450dd2519e285c953593ba7a312da59e4c

# Bridge Hub

## Introduction

The Bridge Hub system parachain plays a crucial role in facilitating trustless interactions between Polkadot, Kusama, Ethereum, and other blockchain ecosystems. By implementing on-chain light clients and supporting protocols like BEEFY and GRANDPA, Bridge Hub ensures seamless message transmission and state verification across chains. It also provides essential [pallets](/polkadot-mkdocs/reference/glossary/#pallet) for sending and receiving messages, making it a cornerstone of Polkadot’s interoperability framework. With built-in support for XCM (Cross-Consensus Messaging), Bridge Hub enables secure, efficient communication between diverse blockchain networks.

This guide covers the architecture, components, and deployment of the Bridge Hub system. You'll explore its trustless bridging mechanisms, key pallets for various blockchains, and specific implementations like Snowbridge and the Polkadot <> Kusama bridge. By the end, you'll understand how Bridge Hub enhances connectivity within the Polkadot ecosystem and beyond.

## Trustless Bridging

Bridge Hub provides a mode of trustless bridging through its implementation of on-chain light clients and trustless relayers. Trustless bridges are essentially two one-way bridges, where each chain has a method of verifying the state of the other in a trustless manner through consensus proofs. In this context, "trustless" refers to the lack of need to trust a human when interacting with various system components. Trustless systems are based instead on trusting mathematics, cryptography, and code. The target chain and source chain both provide ways of verifying one another's state and actions (such as a transfer) based on the consensus and finality of both chains rather than an external mechanism controlled by a third party.

[BEEFY (Bridge Efficiency Enabling Finality Yielder)](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/#bridging-beefy) is instrumental in this solution. It provides a more efficient way to verify the consensus on the relay chain. It allows the participants in a network to verify finality proofs, meaning a remote chain like Ethereum can verify the state of Polkadot at a given block height. 

For example, the Ethereum and Polkadot bridging solution that [Snowbridge](https://docs.snowbridge.network/) implements involves two light clients: one which verifies the state of Polkadot and the other which verifies the state of Ethereum. The light client for Polkadot is implemented in the runtime as a pallet, whereas the light client for Ethereum is implemented as a smart contract on the beacon chain.

## Bridging Components

In any given Bridge Hub implementation (Kusama, Polkadot, or other relay chains), there are a few primary pallets that are utilized:

- **[Pallet Bridge GRANDPA](https://paritytech.github.io/polkadot-sdk/master/pallet_bridge_grandpa/index.html)**: An on-chain GRANDPA light client for Substrate based chains.
- **[Pallet Bridge Parachains](https://paritytech.github.io/polkadot-sdk/master/pallet_bridge_parachains/index.html)**: A finality module for parachains.
- **[Pallet Bridge Messages](https://paritytech.github.io/polkadot-sdk/master/pallet_bridge_messages/index.html)**: A pallet which allows sending, receiving, and tracking of inbound and outbound messages.
- **[Pallet XCM Bridge](https://paritytech.github.io/polkadot-sdk/master/pallet_xcm_bridge_hub/index.html)**: A pallet which, with the Bridge Messages pallet, adds XCM support to bridge pallets.

### Ethereum-Specific Support

Bridge Hub also has a set of components and pallets that support a bridge between Polkadot and Ethereum through [Snowbridge](https://github.com/Snowfork/snowbridge).

To view the complete list of which pallets are included in Bridge Hub, visit the Subscan [Runtime Modules](https://bridgehub-polkadot.subscan.io/runtime) page. Alternatively, the source code for those pallets can be found in the Polkadot SDK [Snowbridge Pallets](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/bridges/snowbridge/pallets) repository.

## Deployed Bridges

- [**Snowbridge**](https://wiki.polkadot.com/learn/learn-snowbridge/): A general-purpose, trustless bridge between Polkadot and Ethereum.
- [**Hyperbridge**](https://wiki.polkadot.com/learn/learn-hyperbridge/): A cross-chain solution built as an interoperability coprocessor, providing state-proof-based interoperability across all blockchains.
- [**Polkadot <> Kusama Bridge**](https://wiki.polkadot.com/learn/learn-dot-ksm-bridge/): A bridge that utilizes relayers to bridge the Polkadot and Kusama relay chains trustlessly.

## Where to Go Next

- Go over the Bridge Hub README in the Polkadot SDK [Bridge-hub Parachains](https://github.com/paritytech/polkadot-sdk/blob/polkadot-stable2512-3/cumulus/parachains/runtimes/bridge-hubs/README.md) repository.
- Take a deeper dive into bridging architecture in the Polkadot SDK [High-Level Bridge](https://github.com/paritytech/polkadot-sdk/blob/polkadot-stable2512-3/bridges/docs/high-level-overview.md) documentation.
- Read more about [BEEFY and Bridging in the Polkadot Wiki](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/#bridging-beefy).


---

Page Title: Chain Data

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/chain-data.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/chain-data/
- Summary: Learn how to expose and utilize chain data for blockchain applications. Discover runtime metadata, RPC APIs, and tools for efficient development.
- Word Count: 2176; Token Estimate: 3653
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:ec51149e9fa0220e2a9ff9e53515049a599dbda51b226031773e17abb54aee84

# Chain Data

## Introduction

Understanding and leveraging on-chain data is a fundamental aspect of blockchain development. Whether you're building frontend applications or backend systems, accessing and decoding runtime metadata is vital to interacting with the blockchain. This guide introduces you to the tools and processes for generating and retrieving metadata, explains its role in application development, and outlines the additional APIs available for interacting with a Polkadot node. By mastering these components, you can ensure seamless communication between your applications and the blockchain.

## Application Development

You might not be directly involved in building frontend applications as a blockchain developer. However, most applications that run on a blockchain require some form of frontend or user-facing client to enable users or other programs to access and modify the data that the blockchain stores. For example, you might develop a browser-based, mobile, or desktop application that allows users to submit transactions, post articles, view their assets, or track previous activity. The backend for that application is configured in the runtime logic for your blockchain, but the frontend client makes the runtime features accessible to your users.

For your custom chain to be useful to others, you'll need to provide a client application that allows users to view, interact with, or update information that the blockchain keeps track of. In this article, you'll learn how to expose information about your runtime so that client applications can use it, see examples of the information exposed, and explore tools and libraries that use it.

## Understand Metadata

Polkadot SDK-based blockchain networks are designed to expose their runtime information, allowing developers to learn granular details regarding pallets, RPC calls, and runtime APIs. The metadata also exposes their related documentation. The chain's metadata is [SCALE-encoded](/polkadot-mkdocs/reference/parachains/data-encoding/), allowing for the development of browser-based, mobile, or desktop applications to support the chain's runtime upgrades seamlessly. It is also possible to develop applications compatible with multiple Polkadot SDK-based chains simultaneously.

## Expose Runtime Information as Metadata

To interact with a node or the state of the blockchain, you need to know how to connect to the chain and access the exposed runtime features. This interaction involves a Remote Procedure Call (RPC) through a node endpoint address, commonly through a secure web socket connection.

An application developer typically needs to know the contents of the runtime logic, including the following details:

- Version of the runtime the application is connecting to.
- Supported APIs.
- Implemented pallets.
- Defined functions and corresponding type signatures.
- Defined custom types.
- Exposed parameters users can set.

As the Polkadot SDK is modular and provides a composable framework for building blockchains, there are limitless opportunities to customize the schema of properties. Each runtime can be configured with its properties, including function calls and types, which can be changed over time with runtime upgrades.

The Polkadot SDK enables you to generate the runtime metadata schema to capture information unique to a runtime. The metadata for a runtime describes the pallets in use and types defined for a specific runtime version. The metadata includes information about each pallet's storage items, functions, events, errors, and constants. The metadata also provides type definitions for any custom types included in the runtime.

Metadata provides a complete inventory of a chain's runtime. It is key to enabling client applications to interact with the node, parse responses, and correctly format message payloads sent back to that chain.

## Generate Metadata

To efficiently use the blockchain's networking resources and minimize the data transmitted over the network, the metadata schema is encoded using the [Parity SCALE Codec](https://github.com/paritytech/parity-scale-codec?tab=readme-ov-file#parity-scale-codec). This encoding is done automatically through the [`scale-info`](https://docs.rs/scale-info/latest/scale_info/)crate.

At a high level, generating the metadata involves the following steps:

1. The pallets in the runtime logic expose callable functions, types, parameters, and documentation that need to be encoded in the metadata.
2. The `scale-info` crate collects type information for the pallets in the runtime, builds a registry of the pallets that exist in a particular runtime, and the relevant types for each pallet in the registry. The type information is detailed enough to enable encoding and decoding for every type.
3. The [`frame-metadata`](https://github.com/paritytech/frame-metadata) crate describes the structure of the runtime based on the registry provided by the `scale-info` crate.
4. Nodes provide the RPC method `state_getMetadata` to return a complete description of all the types in the current runtime as a hex-encoded vector of SCALE-encoded bytes.

## Retrieve Runtime Metadata

The type information provided by the metadata enables applications to communicate with nodes using different runtime versions and across chains that expose different calls, events, types, and storage items. The metadata also allows libraries to generate a substantial portion of the code needed to communicate with a given node, enabling libraries like [`subxt`](https://github.com/paritytech/subxt) to generate frontend interfaces that are specific to a target chain.

### Use Polkadot.js

Visit the [Polkadot.js Portal](https://polkadot.js.org/apps/#/rpc) and select the **Developer** dropdown in the top banner. Select **RPC Calls** to make the call to request metadata. Follow these steps to make the RPC call:

1. Select **state** as the endpoint to call.
2. Select **`getMetadata(at)`** as the method to call.
3. Click **Submit RPC call** to submit the call and return the metadata in JSON format.

### Use Curl 

You can fetch the metadata for the network by calling the node's RPC endpoint. This request returns the metadata in bytes rather than human-readable JSON:

```sh
curl -H "Content-Type: application/json" \
-d '{"id":1, "jsonrpc":"2.0", "method": "state_getMetadata"}' \
https://rpc.polkadot.io

```

### Use Subxt

[`subxt`](https://github.com/paritytech/subxt) may also be used to fetch the metadata of any data in a human-readable JSON format: 

```sh
subxt metadata  --url wss://rpc.polkadot.io --format json > spec.json
```

Another option is to use the [`subxt` explorer web UI](https://paritytech.github.io/subxt-explorer/#/).

## Client Applications and Metadata

The metadata exposes the expected way to decode each type, meaning applications can send, retrieve, and process application information without manual encoding and decoding. Client applications must use the [SCALE codec library](https://github.com/paritytech/parity-scale-codec?tab=readme-ov-file#parity-scale-codec) to encode and decode RPC payloads to use the metadata. Client applications use the metadata to interact with the node, parse responses, and format message payloads sent to the node.

## Metadata Format

Although the SCALE-encoded bytes can be decoded using the `frame-metadata` and [`parity-scale-codec`](https://github.com/paritytech/parity-scale-codec) libraries, there are other tools, such as `subxt` and the Polkadot-JS API, that can convert the raw data to human-readable JSON format.

The types and type definitions included in the metadata returned by the `state_getMetadata` RPC call depend on the runtime's metadata version.

In general, the metadata includes the following information:

- A constant identifying the file as containing metadata.
- The version of the metadata format used in the runtime.
- Type definitions for all types used in the runtime and generated by the `scale-info` crate.
- Pallet information for the pallets included in the runtime in the order that they are defined in the `construct_runtime` macro.

!!!tip 
    Depending on the frontend library used (such as the [Polkadot API](https://papi.how/)), they may format the metadata differently than the raw format shown.

The following example illustrates a condensed and annotated section of metadata decoded and converted to JSON:

```json
[
    1635018093,
    {
        "V14": {
            "types": {
                "types": [{}]
            },
            "pallets": [{}],
            "extrinsic": {
                "ty": 126,
                "version": 4,
                "signed_extensions": [{}]
            },
            "ty": 141
        }
    }
]
```

The constant `1635018093` is a magic number that identifies the file as a metadata file. The rest of the metadata is divided into the `types`, `pallets`, and `extrinsic` sections:

- The `types` section contains an index of the types and information about each type's type signature.
- The `pallets` section contains information about each pallet in the runtime.
- The `extrinsic` section describes the type identifier and transaction format version that the runtime uses.

Different extrinsic versions can have varying formats, especially when considering [signed transactions](/polkadot-mkdocs/reference/parachains/blocks-transactions-fees/transactions/#signed-transactions). 

### Pallets

The following is a condensed and annotated example of metadata for a single element in the `pallets` array (the [`sudo`](https://paritytech.github.io/polkadot-sdk/master/pallet_sudo/index.html) pallet):

```json
{
    "name": "Sudo",
    "storage": {
        "prefix": "Sudo",
        "entries": [
            {
                "name": "Key",
                "modifier": "Optional",
                "ty": {
                    "Plain": 0
                },
                "default": [0],
                "docs": ["The `AccountId` of the sudo key."]
            }
        ]
    },
    "calls": {
        "ty": 117
    },
    "event": {
        "ty": 42
    },
    "constants": [],
    "error": {
        "ty": 124
    },
    "index": 8
}
```

Every element metadata contains the name of the pallet it represents and information about its storage, calls, events, and errors. You can look up details about the definition of the calls, events, and errors by viewing the type index identifier. The type index identifier is the `u32` integer used to access the type information for that item. For example, the type index identifier for calls in the Sudo pallet is 117. If you view information for that type identifier in the `types` section of the metadata, it provides information about the available calls, including the documentation for each call.

For example, the following is a condensed excerpt of the calls for the Sudo pallet:

```json
{
    "id": 117,
    "type": {
        "path": ["pallet_sudo", "pallet", "Call"],
        "params": [
            {
                "name": "T",
                "type": null
            }
        ],
        "def": {
            "variant": {
                "variants": [
                    {
                        "name": "sudo",
                        "fields": [
                            {
                                "name": "call",
                                "type": 114,
                                "typeName": "Box<<T as Config>::RuntimeCall>"
                            }
                        ],
                        "index": 0,
                        "docs": [
                            "Authenticates sudo key, dispatches a function call with `Root` origin"
                        ]
                    },
                    {
                        "name": "sudo_unchecked_weight",
                        "fields": [
                            {
                                "name": "call",
                                "type": 114,
                                "typeName": "Box<<T as Config>::RuntimeCall>"
                            },
                            {
                                "name": "weight",
                                "type": 8,
                                "typeName": "Weight"
                            }
                        ],
                        "index": 1,
                        "docs": [
                            "Authenticates sudo key, dispatches a function call with `Root` origin"
                        ]
                    },
                    {
                        "name": "set_key",
                        "fields": [
                            {
                                "name": "new",
                                "type": 103,
                                "typeName": "AccountIdLookupOf<T>"
                            }
                        ],
                        "index": 2,
                        "docs": [
                            "Authenticates current sudo key, sets the given AccountId (`new`) as the new sudo"
                        ]
                    },
                    {
                        "name": "sudo_as",
                        "fields": [
                            {
                                "name": "who",
                                "type": 103,
                                "typeName": "AccountIdLookupOf<T>"
                            },
                            {
                                "name": "call",
                                "type": 114,
                                "typeName": "Box<<T as Config>::RuntimeCall>"
                            }
                        ],
                        "index": 3,
                        "docs": [
                            "Authenticates sudo key, dispatches a function call with `Signed` origin from a given account"
                        ]
                    }
                ]
            }
        }
    }
}
```

For each field, you can access type information and metadata for the following:

- **Storage metadata**: Provides the information required to enable applications to get information for specific storage items.
- **Call metadata**: Includes information about the runtime calls defined by the `#[pallet]` macro including call names, arguments and documentation.
- **Event metadata**: Provides the metadata generated by the `#[pallet::event]` macro, including the name, arguments, and documentation for each pallet event.
- **Constants metadata**: Provides metadata generated by the `#[pallet::constant]` macro, including the name, type, and hex-encoded value of the constant.
- **Error metadata**: Provides metadata generated by the `#[pallet::error]` macro, including the name and documentation for each pallet error.

!!!tip
    Type identifiers change from time to time, so you should avoid relying on specific type identifiers in your applications.

### Extrinsic

The runtime generates extrinsic metadata and provides useful information about transaction format. When decoded, the metadata contains the transaction version and the list of signed extensions.

For example:

```json
{
    "extrinsic": {
        "ty": 126,
        "version": 4,
        "signed_extensions": [
            {
                "identifier": "CheckNonZeroSender",
                "ty": 132,
                "additional_signed": 41
            },
            {
                "identifier": "CheckSpecVersion",
                "ty": 133,
                "additional_signed": 4
            },
            {
                "identifier": "CheckTxVersion",
                "ty": 134,
                "additional_signed": 4
            },
            {
                "identifier": "CheckGenesis",
                "ty": 135,
                "additional_signed": 11
            },
            {
                "identifier": "CheckMortality",
                "ty": 136,
                "additional_signed": 11
            },
            {
                "identifier": "CheckNonce",
                "ty": 138,
                "additional_signed": 41
            },
            {
                "identifier": "CheckWeight",
                "ty": 139,
                "additional_signed": 41
            },
            {
                "identifier": "ChargeTransactionPayment",
                "ty": 140,
                "additional_signed": 41
            }
        ]
    },
    "ty": 141
}
```

The type system is [composite](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/frame_runtime_types/index.html), meaning each type identifier contains a reference to a specific type or to another type identifier that provides information about the associated primitive types.

For example, you can encode the `BitVec<Order, Store>` type, but to decode it properly, you must know the types used for the `Order` and `Store` types. To find type information for `Order` and `Store`, you can use the path in the decoded JSON to locate their type identifiers.

## Included RPC APIs

A standard node comes with the following APIs to interact with a node:

- **[`AuthorApiServer`](https://paritytech.github.io/polkadot-sdk/master/sc_rpc/author/trait.AuthorApiServer.html)**: Make calls into a full node, including authoring extrinsics and verifying session keys.
- **[`ChainApiServer`](https://paritytech.github.io/polkadot-sdk/master/sc_rpc/chain/trait.ChainApiServer.html)**: Retrieve block header and finality information.
- **[`OffchainApiServer`](https://paritytech.github.io/polkadot-sdk/master/sc_rpc/offchain/trait.OffchainApiServer.html)**: Make RPC calls for off-chain workers.
- **[`StateApiServer`](https://paritytech.github.io/polkadot-sdk/master/sc_rpc/state/trait.StateApiServer.html)**: Query information about on-chain state such as runtime version, storage items, and proofs.
- **[`SystemApiServer`](https://paritytech.github.io/polkadot-sdk/master/sc_rpc/system/trait.SystemApiServer.html)**: Retrieve information about network state, such as connected peers and node roles.

## Additional Resources

The following tools can help you locate and decode metadata:

- [Subxt Explorer](https://paritytech.github.io/subxt-explorer/#/)
- [Metadata Portal 🌗](https://github.com/paritytech/metadata-portal)
- [De[code] Sub[strate]](https://github.com/paritytech/desub)


---

Page Title: Contract Deployment

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/for-eth-devs/contract-deployment.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/for-eth-devs/contract-deployment/
- Summary: Compare deployment flows for REVM and PVM-based smart contracts on the Polkadot Hub. Includes single-step REVM flows and PVM's two-step deployment model.
- Word Count: 754; Token Estimate: 1150
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:b4d64a6b3f0ebc9b16a5cb6d856aea93991b4f807cafeb0b21329924085b0abf

# Contract Deployment

## Introduction

Polkadot's smart contract platform supports two distinct virtual machine backends: Rust Ethereum Virtual Machine (REVM) and PVM. Each backend has its own deployment characteristics and optimization strategies. REVM provides full Ethereum compatibility with familiar single-step deployment, while the RISC-V-based PVM uses a more structured two-step approach optimized for its architecture. Understanding these differences ensures smooth deployment regardless of which backend you choose for your smart contracts.

## REVM Deployment

The REVM backend enables seamless deployment of Ethereum contracts without modification. Contracts deploy exactly as they would on Ethereum, using familiar tools and workflows.

With REVM, deployment mirrors the Ethereum flow exactly including:

- Contracts are bundled and deployed in a single transaction.
- Factory contracts can create new contracts at runtime.
- Runtime code generation, including inline assembly, is supported.
- Existing familiar tools like Hardhat, Foundry, and Remix work out of the box.

## PVM Deployment

PVM implements a fundamentally different deployment model optimized for its RISC-V architecture. While simple contract deployments work seamlessly, advanced patterns like factory contracts require understanding the two-step deployment process.

### Standard Contract Deployment

For most use cases, such as deploying ERC-20 tokens, NFT collections, or standalone contracts, deployment is transparent and requires no special steps. The [Revive compiler](https://github.com/paritytech/revive) handles the deployment process automatically when using standard Solidity patterns.

### Two-Step Deployment Model

PVM separates contract deployment into distinct phases:

1. **Code upload**: Contract bytecode must be uploaded to the chain before instantiation.
2. **Contract instantiation**: Contracts are created by referencing previously uploaded code via its hash.

This architecture differs from the EVM's bundled approach and has important implications for specific deployment patterns.

### Factory Pattern Considerations

The common EVM pattern, where contracts dynamically create other contracts, requires adaptation for PVM as follows:

**EVM Factory Pattern:**
```solidity
// This works on REVM but requires modification for PVM
contract Factory {
    function createToken() public returns (address)
}
```

**PVM Requirements:**

- **Pre-upload dependent contracts**: All contracts that will be instantiated at runtime must be uploaded to the chain before the factory attempts to create them.
- **Code hash references**: Factory contracts work with pre-uploaded code hashes rather than embedding bytecode.
- **No runtime code generation**: Dynamic bytecode generation is not supported due to PVM's RISC-V format.

### Migration Strategy for Factory Contracts

When migrating factory contracts from Ethereum to PVM:

1. **Identify all contracts**: Determine which contracts will be instantiated at runtime.
2. **Upload dependencies first**: Deploy all dependent contracts to the chain before deploying the factory.
3. **Use on-chain constructors**: Leverage PVM's on-chain constructor feature for flexible instantiation.
4. **Avoid assembly creation**: Don't use `create` or `create2` opcodes in assembly blocks for manual deployment.

### Architecture-Specific Limitations

PVM's deployment model creates several specific constraints:

- **`EXTCODECOPY` limitations**: Contracts using `EXTCODECOPY` to manipulate code at runtime will encounter issues.
- **Runtime code modification**: Patterns that construct and mutate contract code on-the-fly are not supported.
- **Assembly-based factories**: Factory contracts written in YUL assembly that generate code at runtime will fail with `CodeNotFound` errors.

These patterns are rare in practice and typically require dropping down to assembly, making them non-issues for standard Solidity development.

### On-Chain Constructors

PVM provides on-chain constructors as an elegant alternative to runtime code modification:

- Enable contract instantiation without runtime code generation.
- Support flexible initialization patterns.
- Maintain separation between code upload and contract creation.
- Provide predictable deployment costs.

## Gas Estimation vs Actual Consumption

Both REVM and PVM deployments may show significant differences between gas estimation and actual consumption. You might see estimates that are several times higher than the actual gas consumed (often around 30% of the estimate). This is normal behavior because pre-dispatch estimation cannot distinguish between computation weight and storage deposits, leading to conservative overestimation. Contract deployments are particularly affected as they consume significant storage deposits for code storage.

## Deployment Comparison

|        Feature        |      REVM Backend       |           PVM Backend           |
|:---------------------:|:-----------------------:|:-------------------------------:|
| **Deployment Model**  |   Single-step bundled   | Two-step upload and instantiate |
| **Factory Patterns**  | Direct runtime creation |   Requires pre-uploaded code    |
|   **Code Bundling**   | Bytecode in transaction |      Code hash references       |
|  **Runtime Codegen**  |     Fully supported     |          Not supported          |
| **Simple Contracts**  | No modifications needed |     No modifications needed     |
| **Assembly Creation** |        Supported        |  Discouraged, limited support   |

## Conclusion

Both backends support contract deployment effectively, with REVM offering drop-in Ethereum compatibility and PVM providing a more structured two-step approach. For the majority of use cases—deploying standard contracts like tokens or applications—both backends work seamlessly. Advanced patterns like factory contracts may require adjustment for PVM, but these adaptations are straightforward with proper planning.


---

Page Title: Create an Account

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/accounts/create-account.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/accounts/create-account/
- Summary: Step-by-step guide to creating Polkadot accounts using different programming languages and libraries, including JavaScript, Python, and Rust examples.
- Word Count: 875; Token Estimate: 1606
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:39075af4108e37fb095f9283e296930092a99e37abc19d68909a0d28810f96ec

# Create an Account

## Introduction

Creating accounts is a fundamental operation when building applications on Polkadot and its parachains. Accounts serve as the basis for identity, asset ownership, and transaction signing. Understanding how to generate and manage accounts programmatically enables you to build wallets, automate operations, and create seamless user experiences.

Polkadot accounts are based on the SR25519 signature scheme by default, though ED25519 and ECDSA are also supported. Each account consists of a public key (address) and a private key (seed/mnemonic). **Keep your private keys secure and never share them**.

This tutorial will guide you through creating accounts using different programming languages and libraries.

## Prerequisites

Before starting, make sure you have:

- Basic understanding of public-key cryptography concepts
- Development environment set up for your chosen language
- Familiarity with the programming language you'll be using

## Use JavaScript/TypeScript

JavaScript/TypeScript developers can use the Polkadot.js API to create and manage Polkadot accounts.

1. Create a new project directory and initialize it:

    ```bash
    mkdir account-creator
    cd account-creator
    npm init -y && npm pkg set type=module
    ```

2. Install the required packages:

    ```bash
    npm install @polkadot/util-crypto @polkadot/keyring
    npm install --save-dev typescript tsx
    ```

3. Create a file named `create-account.ts` and add the following code to it:

    ```typescript title="create-account.ts"
    import { cryptoWaitReady, mnemonicGenerate } from '@polkadot/util-crypto';
    import { Keyring } from '@polkadot/keyring';

    async function main());
      const pair = keyring.addFromMnemonic(mnemonic);

      console.log(`Address: ${pair.address}`);
      console.log(`Mnemonic: ${mnemonic}`);
    }

    main().catch(console.error);
    ```

    Key aspects of the code:

    - **Mnemonic generation**: Uses `mnemonicGenerate()` to create a 12-word BIP39 mnemonic phrase for human-readable key backup.
    - **Keyring**: The `Keyring` class manages accounts with a specified signature scheme and address format.
    - **SS58 format**: Setting `ss58Format: 0` configures addresses for the Polkadot relay chain.

4. Execute the script using `tsx`:

    ```bash
    npx tsx create-account.ts
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>npx tsx create-account.ts</span>
        <span data-ty="progress"></span>
        <span data-ty>Address: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty>Mnemonic: cushion dog echo people vendor curve truck begin latin romance rebuild ...</span>
    </div>
## Python

Python developers can use the `substrate-interface` library to create and manage Polkadot accounts.

1. Create a new project directory and set up a virtual environment:

    ```bash
    mkdir account-creator-python
    cd account-creator-python
    python3 -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

2. Install the required package:

    ```bash
    pip install substrate-interface
    ```

3. Create a file named `create_account.py`:

    ```python title="create_account.py"
    from substrateinterface import Keypair

    mnemonic = Keypair.generate_mnemonic()
    keypair = Keypair.create_from_mnemonic(mnemonic)

    print(f"Address: {keypair.ss58_address}")
    print(f"Mnemonic: {mnemonic}")
    ```

    Key aspects of the code:

    - **Mnemonic generation**: The `generate_mnemonic()` function creates a BIP39-compatible phrase.
    - **Keypair creation**: `Keypair.create_from_mnemonic()` derives keys from the mnemonic.

4. Execute the script:

    ```bash
    python create_account.py
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>python create_account.py</span>
        <span data-ty>Address: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty>Mnemonic: cushion dog echo people vendor curve truck begin latin romance rebuild ...</span>
    </div>
## Rust

Rust provides low-level access to Substrate primitives for account creation through the `sp-core` and `sp-keyring` crates.

1. Create a new Rust project:

    ```bash
    cargo new account-creator-rust
    cd account-creator-rust
    ```

2. Add dependencies to your `Cargo.toml`:

    ```toml title="Cargo.toml"
    [package]
    name = "account-creator-rust"
    version = "0.1.0"
    edition = "2021"

    [dependencies]
    sp-core = "28.0"
    sp-runtime = "31.0"
    ```

3. Create your account generation code in `src/main.rs`:

    ```rust title="src/main.rs"
    use sp_core::{crypto::Ss58Codec, Pair};

    fn main()", address);
        println!("Mnemonic: {}", phrase);
    }
    ```

    Key aspects of the code:

    - **Keypair generation**: [`sr25519::Pair::generate_with_phrase()`](https://docs.rs/sp-core/latest/sp_core/crypto/trait.Pair.html#method.generate_with_phrase) creates a new key pair with mnemonic.
    - **Public key extraction**: The [`public()`](https://docs.rs/sp-core/latest/sp_core/crypto/trait.Pair.html#tymethod.public) method retrieves the public key from the pair.
    - **SS58 encoding**: Uses Polkadot's address format for the human-readable address.

4. Build and run the project:

    ```bash
    cargo run
    ```

    You should see output similar to:

    <div class="termynal" data-termynal>
        <span data-ty="input"><span class="file-path"></span>cargo run</span>
        <span data-ty>Address: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5</span>
        <span data-ty>Mnemonic: cushion dog echo people vendor curve truck begin latin romance rebuild ...</span>
    </div>
## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Send Transactions with SDKs__

    ---

    Learn how to send signed transactions using your newly created accounts with Polkadot-API and Polkadot.js API libraries.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/with-sdks/)

-   <span class="badge guide">Guide</span> __Calculate Transaction Fees__

    ---

    Learn how to estimate transaction fees before sending transactions from your accounts.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/calculate-transaction-fees/)

-   <span class="badge guide">Guide</span> __Query Chain Data__

    ---

    Explore different methods for querying blockchain data, including account balances and other chain state.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/query-data/query-sdks/)

</div>


---

Page Title: Cryptography

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/cryptography.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/cryptography/
- Summary: A concise guide to cryptography in blockchain, covering hash functions, encryption types, digital signatures, and elliptic curve applications.
- Word Count: 1267; Token Estimate: 1721
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:e4e5e1ec65d7ba78ce9b59e753c1707ad7e16b50de9e4d8d0f9c5940d57f6095

# Cryptography

## Introduction

Cryptography forms the backbone of blockchain technology, providing the mathematical verifiability crucial for consensus systems, data integrity, and user security. While a deep understanding of the underlying mathematical processes isn't necessary for most blockchain developers, grasping the fundamental applications of cryptography is essential. This page comprehensively overviews cryptographic implementations used across Polkadot SDK-based chains and the broader blockchain ecosystem.

## Hash Functions

Hash functions are fundamental to blockchain technology, creating a unique digital fingerprint for any piece of data, including simple text, images, or any other form of file. They map input data of any size to a fixed-size output (typically 32 bytes) using complex mathematical operations. Hashing is used to verify data integrity, create digital signatures, and provide a secure way to store passwords. This form of mapping is known as the ["pigeonhole principle,"](https://en.wikipedia.org/wiki/Pigeonhole_principle) it is primarily implemented to efficiently and verifiably identify data from large sets.

### Key Properties of Hash Functions

- **Deterministic**: The same input always produces the same output.
- **Quick computation**: It's easy to calculate the hash value for any given input.
- **Pre-image resistance**: It's infeasible to generate the input data from its hash.
- **Small changes in input yield large changes in output**: Known as the ["avalanche effect"](https://en.wikipedia.org/wiki/Avalanche_effect).
- **Collision resistance**: The probabilities are extremely low to find two different inputs with the same hash.

### Blake2

The Polkadot SDK utilizes Blake2, a state-of-the-art hashing method that offers:

- Equal or greater security compared to [SHA-2](https://en.wikipedia.org/wiki/SHA-2).
- Significantly faster performance than other algorithms.

These properties make Blake2 ideal for blockchain systems, reducing sync times for new nodes and lowering the resources required for validation. For detailed technical specifications about Blake2, see the [official Blake2 paper](https://www.blake2.net/blake2.pdf).

## Types of Cryptography

There are two different ways that cryptographic algorithms are implemented: symmetric cryptography and asymmetric cryptography.

### Symmetric Cryptography

Symmetric encryption is a branch of cryptography that isn't based on one-way functions, unlike asymmetric cryptography. It uses the same cryptographic key to encrypt plain text and decrypt the resulting ciphertext.

Symmetric cryptography is a type of encryption that has been used throughout history, such as the Enigma Cipher and the Caesar Cipher. It is still widely used today and can be found in Web2 and Web3 applications alike. There is only one single key, and a recipient must also have access to it to access the contained information.

#### Advantages {: #symmetric-advantages }

- Fast and efficient for large amounts of data.
- Requires less computational power.

#### Disadvantages {: #symmetric-disadvantages }

- Key distribution can be challenging.
- Scalability issues in systems with many users.

### Asymmetric Cryptography

Asymmetric encryption is a type of cryptography that uses two different keys, known as a keypair: a public key, used to encrypt plain text, and a private counterpart, used to decrypt the ciphertext.

The public key encrypts a fixed-length message that can only be decrypted with the recipient's private key and, sometimes, a set password. The public key can be used to cryptographically verify that the corresponding private key was used to create a piece of data without compromising the private key, such as with digital signatures. This has obvious implications for identity, ownership, and properties and is used in many different protocols across Web2 and Web3.

#### Advantages {: #asymmetric-advantages }

- Solves the key distribution problem.
- Enables digital signatures and secure key exchange.

#### Disadvantages {: #asymmetric-disadvantages }

- Slower than symmetric encryption.
- Requires more computational resources.

### Trade-offs and Compromises

Symmetric cryptography is faster and requires fewer bits in the key to achieve the same level of security that asymmetric cryptography provides. However, it requires a shared secret before communication can occur, which poses issues to its integrity and a potential compromise point. On the other hand, asymmetric cryptography doesn't require the secret to be shared ahead of time, allowing for far better end-user security.

Hybrid symmetric and asymmetric cryptography is often used to overcome the engineering issues of asymmetric cryptography, as it is slower and requires more bits in the key to achieve the same level of security. It encrypts a key and then uses the comparatively lightweight symmetric cipher to do the "heavy lifting" with the message.

## Digital Signatures

Digital signatures are a way of verifying the authenticity of a document or message using asymmetric keypairs. They are used to ensure that a sender or signer's document or message hasn't been tampered with in transit, and for recipients to verify that the data is accurate and from the expected sender.

Signing digital signatures only requires a low-level understanding of mathematics and cryptography. For a conceptual example -- when signing a check, it is expected that it cannot be cashed multiple times. This isn't a feature of the signature system but rather the check serialization system. The bank will check that the serial number on the check hasn't already been used. Digital signatures essentially combine these two concepts, allowing the signature to provide the serialization via a unique cryptographic fingerprint that cannot be reproduced.

Unlike pen-and-paper signatures, knowledge of a digital signature cannot be used to create other signatures. Digital signatures are often used in bureaucratic processes, as they are more secure than simply scanning in a signature and pasting it onto a document.

Polkadot SDK provides multiple different cryptographic schemes and is generic so that it can support anything that implements the [`Pair` trait](https://paritytech.github.io/polkadot-sdk/master/sp_core/crypto/trait.Pair.html).

### Example of Creating a Digital Signature

The process of creating and verifying a digital signature involves several steps:

1. The sender creates a hash of the message.
2. The hash is encrypted using the sender's private key, creating the signature.
3. The message and signature are sent to the recipient.
4. The recipient decrypts the signature using the sender's public key.
5. The recipient hashes the received message and compares it to the decrypted hash.

If the hashes match, the signature is valid, confirming the message's integrity and the sender's identity.

## Elliptic Curve

Blockchain technology requires the ability to have multiple keys creating a signature for block proposal and validation. To this end, Elliptic Curve Digital Signature Algorithm (ECDSA) and Schnorr signatures are two of the most commonly used methods. While ECDSA is a far simpler implementation, Schnorr signatures are more efficient when it comes to multi-signatures.

Schnorr signatures bring some noticeable features over the ECDSA/EdDSA schemes:

- It is better for hierarchical deterministic key derivations.
- It allows for native multi-signature through [signature aggregation](https://bitcoincore.org/en/2017/03/23/schnorr-signature-aggregation/).
- It is generally more resistant to misuse.

One sacrifice that is made when using Schnorr signatures over ECDSA is that both require 64 bytes, but only ECDSA signatures communicate their public key.

### Various Implementations

- **[ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)**: Polkadot SDK provides an ECDSA signature scheme using the [secp256k1](https://en.bitcoin.it/wiki/Secp256k1) curve. This is the same cryptographic algorithm used to secure [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin) and [Ethereum](https://en.wikipedia.org/wiki/Ethereum).

- **[Ed25519](https://en.wikipedia.org/wiki/EdDSA#Ed25519)**: An EdDSA signature scheme using [Curve25519](https://en.wikipedia.org/wiki/Curve25519). It is carefully engineered at several levels of design and implementation to achieve very high speeds without compromising security.

- **[SR25519](https://research.web3.foundation/Polkadot/security/keys/accounts-more)**: Based on the same underlying curve as Ed25519. However, it uses Schnorr signatures instead of the EdDSA scheme.


---

Page Title: Data Encoding

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/data-encoding.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/data-encoding/
- Summary: SCALE codec enables fast, efficient data encoding, ideal for resource-constrained environments like Wasm, supporting custom types and compact encoding.
- Word Count: 1263; Token Estimate: 3065
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:1c6090a30d51851cbcabcb41f73bbe56349770e6c62e978184677e94df3548db

# Data Encoding

## Introduction

The Polkadot SDK uses a lightweight and efficient encoding/decoding mechanism to optimize data transmission across the network. This mechanism, known as the _SCALE_ codec, is used for serializing and deserializing data.

The SCALE codec enables communication between the runtime and the outer node. This mechanism is designed for high-performance, copy-free data encoding and decoding in resource-constrained environments like the Polkadot SDK Wasm runtime.

It is not self-describing, meaning the decoding context must fully know the encoded data types. 

Parity's libraries utilize the [`parity-scale-codec`](https://github.com/paritytech/parity-scale-codec) crate (a Rust implementation of the SCALE codec) to handle encoding and decoding for interactions between RPCs and the runtime.

The `codec` mechanism is ideal for Polkadot SDK-based chains because:

- It is lightweight compared to generic serialization frameworks like [`serde`](https://serde.rs/), which add unnecessary bulk to binaries.
- It doesn’t rely on Rust’s `libstd`, making it compatible with `no_std` environments like Wasm runtime.
- It integrates seamlessly with Rust, allowing easy derivation of encoding and decoding logic for new types using `#[derive(Encode, Decode)]`.

Defining a custom encoding scheme in the Polkadot SDK-based chains, rather than using an existing Rust codec library, is crucial for enabling cross-platform and multi-language support. 

## SCALE Codec

The codec is implemented using the following traits:

- [`Encode`](#encode)
- [`Decode`](#decode)
- [`CompactAs`](#compactas)
- [`HasCompact`](#hascompact)
- [`EncodeLike`](#encodelike)

### Encode

The [`Encode`](https://docs.rs/parity-scale-codec/latest/parity_scale_codec/trait.Encode.html) trait handles data encoding into SCALE format and includes the following key functions:

- **`size_hint(&self) -> usize`**: Estimates the number of bytes required for encoding to prevent multiple memory allocations. This should be inexpensive and avoid complex operations. Optional if the size isn’t known.
- **`encode_to<T: Output>(&self, dest: &mut T)`**: Encodes the data, appending it to a destination buffer.
- **`encode(&self) -> Vec<u8>`**: Encodes the data and returns it as a byte vector.
- **`using_encoded<R, F: FnOnce(&[u8]) -> R>(&self, f: F) -> R`**: Encodes the data and passes it to a closure, returning the result.
- **`encoded_size(&self) -> usize`**: Calculates the encoded size. Should be used when the encoded data isn’t required.

!!!tip
    For best performance, value types should override `using_encoded`, and allocating types should override `encode_to`. It's recommended to implement `size_hint` for all types where possible.

### Decode

The [`Decode`](https://docs.rs/parity-scale-codec/latest/parity_scale_codec/trait.Decode.html) trait handles decoding SCALE-encoded data back into the appropriate types:

- **`fn decode<I: Input>(value: &mut I) -> Result<Self, Error>`**: Decodes data from the SCALE format, returning an error if decoding fails.

### CompactAs

The [`CompactAs`](https://docs.rs/parity-scale-codec/latest/parity_scale_codec/trait.CompactAs.html) trait wraps custom types for compact encoding:

- **`encode_as(&self) -> &Self::As`**: Encodes the type as a compact type.
- **`decode_from(_: Self::As) -> Result<Self, Error>`**: decodes from a compact encoded type.

### HasCompact

The [`HasCompact`](https://docs.rs/parity-scale-codec/latest/parity_scale_codec/trait.HasCompact.html) trait indicates a type supports compact encoding.

### EncodeLike

The [`EncodeLike`](https://docs.rs/parity-scale-codec/latest/parity_scale_codec/trait.EncodeLike.html) trait is used to ensure multiple types that encode similarly are accepted by the same function. When using `derive`, it is automatically implemented.

### Data Types

The table below outlines how the Rust implementation of the Parity SCALE codec encodes different data types.

| Type                          | Description                                                                                                                                                                                                                                                                                                                | Example SCALE Decoded Value                                                                                                                        | SCALE Encoded Value                                                     |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| Boolean                       | Boolean values are encoded using the least significant bit of a single byte.                                                                                                                                                                                                                                               | `false` / `true`                                                                                                                                   | `0x00` / `0x01`                                                         |
| Compact/general integers      | A "compact" or general integer encoding is sufficient for encoding large integers (up to 2^536) and is more efficient at encoding most values than the fixed-width version.                                                                                                                                                | `unsigned integer 0` / `unsigned integer 1` / `unsigned integer 42` / `unsigned integer 69` / `unsigned integer 65535` / `BigInt(100000000000000)` | `0x00` / `0x04` / `0xa8` / `0x1501` / `0xfeff0300` / `0x0b00407a10f35a` |
| Enumerations (tagged-unions)  | A fixed number of variants, each mutually exclusive and potentially implying a further value or series of values. Encoded as the first byte identifying the index of the variant that the value is. Any further bytes are used to encode any data that the variant implies. Thus, no more than 256 variants are supported. | `Int(42)` and `Bool(true)` where `enum IntOrBool { Int(u8), Bool(bool) }`                                                                          | `0x002a` and `0x0101`                                                   |
| Fixed-width integers          | Basic integers are encoded using a fixed-width little-endian (LE) format.                                                                                                                                                                                                                                                  | `signed 8-bit integer 69` / `unsigned 16-bit integer 42` / `unsigned 32-bit integer 16777215`                                                      | `0x45` / `0x2a00` / `0xffffff00`                                        |
| Options                       | One or zero values of a particular type.                                                                                                                                                                                                                                                                                   | `Some` / `None`                                                                                                                                    | `0x01` followed by the encoded value / `0x00`                           |
| Results                       | Results are commonly used enumerations which indicate whether certain operations were successful or unsuccessful.                                                                                                                                                                                                          | `Ok(42)` / `Err(false)`                                                                                                                            | `0x002a` / `0x0100`                                                     |
| Strings                       | Strings are Vectors of bytes (Vec<u8>) containing a valid UTF8 sequence.                                                                                                                                                                                                                                                   |                                                                                                                                                    |                                                                         |
| Structs                       | For structures, the values are named, but that is irrelevant for the encoding (names are ignored - only order matters).                                                                                                                                                                                                    | `SortedVecAsc::from([3, 5, 2, 8])`                                                                                                                 | `[3, 2, 5, 8] `                                                         |
| Tuples                        | A fixed-size series of values, each with a possibly different but predetermined and fixed type. This is simply the concatenation of each encoded value.                                                                                                                                                                    | Tuple of compact unsigned integer and boolean: `(3, false)`                                                                                        | `0x0c00`                                                                |
| Vectors (lists, series, sets) | A collection of same-typed values is encoded, prefixed with a compact encoding of the number of items, followed by each item's encoding concatenated in turn.                                                                                                                                                              | Vector of unsigned `16`-bit integers: `[4, 8, 15, 16, 23, 42]`                                                                                     | `0x18040008000f00100017002a00`                                          |

## Encode and Decode Rust Trait Implementations

Here's how the `Encode` and `Decode` traits are implemented:


```rust
use parity_scale_codec::{Encode, Decode};

[derive(Debug, PartialEq, Encode, Decode)]
enum EnumType {
    #[codec(index = 15)]
    A,
    B(u32, u64),
    C {
        a: u32,
        b: u64,
    },
}

let a = EnumType::A;
let b = EnumType::B(1, 2);
let c = EnumType::C { a: 1, b: 2 };

a.using_encoded(|ref slice| {
    assert_eq!(slice, &b"\x0f");
});

b.using_encoded(|ref slice| {
    assert_eq!(slice, &b"\x01\x01\0\0\0\x02\0\0\0\0\0\0\0");
});

c.using_encoded(|ref slice| {
    assert_eq!(slice, &b"\x02\x01\0\0\0\x02\0\0\0\0\0\0\0");
});

let mut da: &[u8] = b"\x0f";
assert_eq!(EnumType::decode(&mut da).ok(), Some(a));

let mut db: &[u8] = b"\x01\x01\0\0\0\x02\0\0\0\0\0\0\0";
assert_eq!(EnumType::decode(&mut db).ok(), Some(b));

let mut dc: &[u8] = b"\x02\x01\0\0\0\x02\0\0\0\0\0\0\0";
assert_eq!(EnumType::decode(&mut dc).ok(), Some(c));

let mut dz: &[u8] = &[0];
assert_eq!(EnumType::decode(&mut dz).ok(), None);
```

## SCALE Codec Libraries

Several SCALE codec implementations are available in various languages. Here's a list of them:

- **AssemblyScript**: [`LimeChain/as-scale-codec`](https://github.com/LimeChain/as-scale-codec)
- **C**: [`MatthewDarnell/cScale`](https://github.com/MatthewDarnell/cScale)
- **C++**: [`qdrvm/scale-codec-cpp`](https://github.com/qdrvm/scale-codec-cpp)
- **JavaScript**: [`polkadot-js/api`](https://github.com/polkadot-js/api)
- **Dart**: [`leonardocustodio/polkadart`](https://github.com/leonardocustodio/polkadart)
- **Haskell**: [`airalab/hs-web3`](https://github.com/airalab/hs-web3/tree/master/packages/scale)
- **Golang**: [`itering/scale.go`](https://github.com/itering/scale.go)
- **Java**: [`splix/polkaj`](https://github.com/splix/polkaj)
- **Python**: [`polkascan/py-scale-codec`](https://github.com/polkascan/py-scale-codec)
- **Ruby**: [` wuminzhe/scale_rb`](https://github.com/wuminzhe/scale_rb)
- **TypeScript**: [`parity-scale-codec-ts`](https://github.com/tjjfvi/subshape), [`scale-ts`](https://github.com/unstoppablejs/unstoppablejs/tree/main/packages/scale-ts#scale-ts), [`soramitsu/scale-codec-js-library`](https://github.com/soramitsu/scale-codec-js-library), [`subsquid/scale-codec`](https://github.com/subsquid/squid-sdk/tree/master/substrate/scale-codec)


---

Page Title: Deploy an ERC-20 Using Hardhat

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-hardhat.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-hardhat/
- Summary: Deploy an ERC-20 token on Polkadot Hub using PVM. This guide covers contract creation, compilation, deployment, and interaction via Hardhat.
- Word Count: 1093; Token Estimate: 2058
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:8fe76718aaab4f6d366a15590c32bb6ac8daf03d3f3a8bd2f9a25904a402b9a8

# Deploy an ERC-20 Using Hardhat

## Introduction

[ERC-20](https://eips.ethereum.org/EIPS/eip-20) tokens are fungible tokens commonly used for creating cryptocurrencies, governance tokens, and staking mechanisms. Polkadot Hub enables easy deployment of ERC-20 tokens via Ethereum-compatible smart contracts and tools.

This guide demonstrates how to deploy an ERC-20 contract on Polkadot Hub TestNet using [Hardhat](https://hardhat.org/), an Ethereum development environment. The ERC-20 contract can be retrieved from OpenZeppelin's [GitHub repository](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/v5.6.1/contracts/token/ERC20) or generated with the [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot).

## Prerequisites

Before you begin, ensure you have the following:

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming and [ERC-20](https://ethereum.org/developers/docs/standards/tokens/erc-20/) fungible tokens.
- [Node.js](https://nodejs.org/en/download) v22.13.1 or later installed.
- Test tokens for gas fees, available from the [Polkadot faucet](https://faucet.polkadot.io/). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.
- A wallet with a private key for signing transactions.

## Set Up Your Project

This tutorial uses a [Hardhat ERC-20 template](https://github.com/polkadot-developers/revm-hardhat-examples/tree/master/erc20-hardhat) that contains all the necessary files. 

To get started, take the following steps:

1. Clone the GitHub repository locally:

    ```bash
    git clone https://github.com/polkadot-developers/revm-hardhat-examples/
    cd revm-hardhat-examples/erc20-hardhat
    ```

2. Install the dependencies using the following command:

    ```bash
    npm i
    ```
    
    This command will fetch all the necessary packages to help you use Hardhat to deploy an ERC-20 to Polkadot.

## Configure Hardhat

If you started with the cloned Hardhat ERC-20 template, `hardhat.config.ts` is already configured to deploy to the Polkadot TestNet as shown in the example below:

```ts title="hardhat.config.ts" hl_lines="14-19"
import { HardhatUserConfig, vars } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.28",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
  networks: {
    polkadotTestnet: {
      url: "https://services.polkadothub-rpc.com/testnet",
      accounts: vars.has("TESTNET_PRIVATE_KEY") ? [vars.get("TESTNET_PRIVATE_KEY")] : [],
    },
  },
  mocha: {
    timeout: 40000,
  },
};

export default config;
```

!!! tip
    Visit the Hardhat [Configuration variables](https://v2.hardhat.org/hardhat-runner/docs/guides/configuration-variables) documentation to learn how to use Hardhat to handle your private keys securely.

## Compile the Contract 

Next, compile the contract included with the template by running the following command:

```bash
npx hardhat compile
```

If everything compiles successfully, you will see output similar to the following:

<div id="termynal" data-termynal markdown>
  <span data-ty="input">npx hardhat compile</span>
  <span data-ty>Generating typings for: 23 artifacts in dir: typechain-types for target: ethers-v6</span>
  <span data-ty>Successfully generated 62 typings!</span>
  <span data-ty>Compiled 21 Solidity files successfully (evm target: paris).</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
## Test the Contract

You can view the predefined test file at [`test/MyToken.test.ts`](https://github.com/polkadot-developers/revm-hardhat-examples/blob/master/erc20-hardhat/test/MyToken.test.ts). This example test includes verification of the following:

- The token name and symbol exist (confirms deployment) and are correct.
- The token owner is correctly configured.
- The initial token supply is zero.
- The owner can mint tokens.
- The total supply increases after a mint.
- Successful mints to different test addresses with expected account balance and total supply changes.

Run the tests using the following command:

```bash
npx hardhat test --network polkadotTestnet
```

If tests are successful, you will see outputs similar to the following:

<div id="termynal" data-termynal markdown>
  <span data-ty="input">npx hardhat test --network polkadotTestnet</span>
  <span data-ty></span>
  <span data-ty>&nbsp;&nbsp;MyToken</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;Deployment</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should have correct name and symbol</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should set the right owner</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should have zero initial supply</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;Minting</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should allow owner to mint tokens</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should increase total supply on mint</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;Multiple mints</span>
  <span data-ty>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✔ Should correctly track balance after multiple mints</span>
  <span data-ty></span>
  <span data-ty>&nbsp;&nbsp;6 passing (369ms)</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
## Deploy the Contract

You are now ready to deploy the contract to your chosen network. This example demonstrates deployment to the Polkadot TestNet. Deploy the contract as follows:

1. Run the following command in your terminal:

    ```bash
    npx hardhat ignition deploy ./ignition/modules/MyToken.ts --network polkadotTestnet
    ```

2. Confirm the target deployment network name and chain ID when prompted:

    <div id="termynal" data-termynal markdown>
      <span data-ty="input">npx hardhat ignition deploy ./ignition/modules/MyToken.ts --network polkadotTestnet</span>
      <span data-ty>✔ Confirm deploy to network polkadotTestnet (420420417)? … yes</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Hardhat Ignition 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deploying [ TokenModule ]</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Batch #1</span>
      <span data-ty> Executed TokenModule#MyToken</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Batch #2</span>
      <span data-ty> Executed TokenModule#MyToken.mint</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>[ TokenModule ] successfully deployed 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deployed Addresses</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>TokenModule#MyToken - 0xc01Ee7f10EA4aF4673cFff62710E1D7792aBa8f3</span>
      <span data-ty="input"><span class="file-path"></span></span>
    </div>
Congratulations! You've successfully deployed an ERC-20 token contract to Polkadot Hub TestNet using Hardhat. Consider the following resources to build upon your progress.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an NFT__

    ---

    Walk through deploying an NFT to the Polkadot Hub using Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-hardhat/)

-   <span class="badge guide">Guide</span> __Create a DApp__

    ---

    Learn step-by-step how to build a fully functional dApp that interacts with a smart contract deployed via Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/dapps/zero-to-hero/)

</div>


---

Page Title: Deploy an ERC-20 Using Remix IDE

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/
- Summary: Deploy an ERC-20 token contract on Polkadot Hub. This guide covers contract creation, compilation, deployment, and interaction via the Remix IDE.
- Word Count: 879; Token Estimate: 1430
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:c4410cd05d44b29483176c2f706014aba11d3b0c68bfe5a7b23d9f412dc5423c

# Deploy an ERC-20 Using Remix IDE

## Introduction

[ERC-20](https://eips.ethereum.org/EIPS/eip-20) tokens are fungible tokens commonly used for creating cryptocurrencies, governance tokens, and staking mechanisms. Polkadot Hub enables easy token deployment with Ethereum-compatible smart contracts and tools via the EVM backend.

This tutorial covers deploying an ERC-20 contract on Polkadot Hub TestNet using [Remix IDE](https://remix.ethereum.org/), a web-based development tool. The ERC-20 contract can be retrieved from OpenZeppelin's [GitHub repository](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/v5.6.1/contracts/token/ERC20) or generated with the [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot).

## Prerequisites

Before you begin, ensure you have:

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming and [ERC-20](https://ethereum.org/developers/docs/standards/tokens/erc-20/) fungible tokens.
- An EVM-compatible [wallet](/polkadot-mkdocs/smart-contracts/connect/) connected to Polkadot Hub. This example utilizes [MetaMask](https://metamask.io/).
- Test tokens for gas fees, available from the [Polkadot faucet](https://faucet.polkadot.io/). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.

## Create Your Contract

Follow the steps below to create the ERC-20 contract:

1. Navigate to [Remix IDE](https://remix.ethereum.org/) in your web browser.
2. Select the **Create new file** button under the **contracts** folder, and name your contract `MyToken.sol`.

    ![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/remix-01.webp)

3. Now, paste the following ERC-20 contract code into `MyToken.sol`:

    ```solidity title="MyToken.sol"
    // SPDX-License-Identifier: MIT
    // Compatible with OpenZeppelin Contracts ^5.4.0
    pragma solidity ^0.8.27;

    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import {ERC20Permit} from "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";
    import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

    contract MyToken is ERC20, Ownable, ERC20Permit {
        constructor(address initialOwner)
            ERC20("MyToken", "MTK")
            Ownable(initialOwner)
            ERC20Permit("MyToken")
        {}

        function mint(address to, uint256 amount) public onlyOwner {
            _mint(to, amount);
        }
    }
    ```
    
    !!! tip
        The [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot) was used to generate this example ERC-20 contract. Use it to customize and generate your own ERC-20, ERC-721, or other OpenZeppelin-standard contracts for Polkadot Hub.
        
## Compile the Contract

Solidity source code compiles into bytecode that can be deployed on the blockchain. During this process, the compiler checks the contract for syntax errors, ensures type safety, and generates the machine-readable instructions needed for blockchain execution.

Ensure your `MyToken.sol` contract is open in the Remix IDE Editor, and use the following steps to compile:

1. Select the **Solidity Compiler** plugin from the left panel.
2. Select the **Compile MyToken.sol** button.

The **Solidity Compiler** icon will display a green checkmark once the contract compiles successfully. If any issues arise during contract compilation, errors and warnings will appear in the terminal panel at the bottom of the screen.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/remix-03.gif)

## Deploy the Contract

Follow these steps to deploy the contract using Remix:

1. Select **Deploy & Run Transactions** from the left panel.
2. Ensure your MetaMask wallet is connected to Polkadot Hub TestNet, then select the **Environment** dropdown and select **Injected Provider - MetaMask**.
3. Configure the contract parameters by entering the address that will own the deployed token contract.
4. Select the **Deploy** button to initiate the deployment.
4. Approve the transaction in your MetaMask wallet when prompted.
6. You will see the transaction details in the terminal when the deployment succeeds, including the contract address and deployment transaction hash.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/remix-04.gif)

Once successfully deployed, your contract will appear in the **Deployed Contracts** section, ready for interaction.

## Interact with the Contract

Once deployed, you can interact with your contract through Remix. Find your contract under **Deployed/Unpinned Contracts**, and select it to expand the available methods. In this example, you'll mint some tokens to a given address using the following steps:

1. Expand the **mint** function, then enter the recipient address and the amount (remember to add 18 zeros for one whole token).
2. Select **transact**.
3. Approve the transaction in your MetaMask wallet when prompted.
4. You will see a green check mark in the terminal when the transaction is successful.
5. You can also call the **balanceOf** function by passing the address of the **mint** call to confirm the new balance.

![](/polkadot-mkdocs/images/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/remix-05.gif)

Feel free to explore and interact with the contract's other functions by selecting the method, providing any required parameters, and confirming the transaction in MetaMask when prompted.

Congratulations! You've successfully deployed an ERC-20 token contract to Polkadot Hub TestNet using Remix IDE. Consider the following resources to build upon your progress. 

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an NFT with Remix__

    ---

    Walk through deploying an ERC-721 Non-Fungible Token (NFT) using OpenZeppelin's battle-tested NFT implementation and Remix.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/)

</div>


---

Page Title: Deploy an ERC-721 NFT Using Remix

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/
- Summary: Learn how to deploy an ERC-721 NFT contract to Polkadot Hub using Remix, a browser-based IDE for quick prototyping and learning.
- Word Count: 689; Token Estimate: 1167
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:464fc3ffeecef8527fb0c55638ac6cd79e81dd9ca9d7faca40ac6055cc89f5b1

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


---

Page Title: Deploy an ERC-721 Using Hardhat

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-hardhat.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-hardhat/
- Summary: Learn how to deploy an ERC-721 NFT contract to Polkadot Hub using Hardhat, a comprehensive development environment with built-in deployment capabilities.
- Word Count: 984; Token Estimate: 1842
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:e00dccaf2062e05f1c6c63e8f80c2b638948999c6bffe7e291d8410e39e44c6f

# Deploy an ERC-721 Using Hardhat

## Introduction

Non-Fungible Tokens (NFTs) represent unique digital assets commonly used for digital art, collectibles, gaming, and identity verification.

This guide demonstrates how to deploy an [ERC-721](https://eips.ethereum.org/EIPS/eip-721) NFT contract to [Polkadot Hub](/polkadot-mkdocs/smart-contracts/overview/#smart-contract-development) TestNet. You'll use OpenZeppelin's battle-tested [NFT implementation](https://github.com/OpenZeppelin/openzeppelin-contracts) and [Hardhat](https://hardhat.org/docs/getting-started), a comprehensive development environment with built-in testing, debugging, and deployment capabilities. You can generate a custom NFT contract with the [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot), then use it in this Hardhat workflow. Hardhat uses standard Solidity compilation to generate EVM bytecode, making it fully compatible with Polkadot Hub's EVM environment.

## Prerequisites

Before you begin, ensure you have the following:

- A basic understanding of [Solidity](https://www.soliditylang.org/) programming and [ERC-721](https://ethereum.org/developers/docs/standards/tokens/erc-721/) non-fungible tokens.
- [Node.js](https://nodejs.org/en/download) v22.13.1 or later installed.
- Test tokens for gas fees, available from the [Polkadot faucet](https://faucet.polkadot.io/). See [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/#get-test-tokens) for a guide to using the faucet.
- A wallet with a private key for signing transactions.

## Set Up Your Project

1. Use the following terminal commands to create a directory and initialize your Hardhat project inside of it:

    ```bash
    mkdir hardhat-nft-deployment
    cd hardhat-nft-deployment
    npx hardhat@^2.27.0 init
    ```

2. Install the OpenZeppelin contract dependencies using the command:

    ```bash
    npm install @openzeppelin/contracts
    ```

## Configure Hardhat

Open `hardhat.config.ts` and update to add `polkadotTestnet` to the `networks` configuration as highlighted in the following example code:

```typescript title="hardhat.config.ts" hl_lines='18-23'
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

2. Create a new file named `MyNFT.sol` inside the `contracts` directory.

3. Add the following code to create the `MyNFT.sol` smart contract:
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

## Compile the Contract

Compile your `MyNFT.sol` contract using the following command:

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

2. Create a new file named `MyNFT.ts` inside the `ignition/modules` directory.

3. Open `ignition/modules/MyNFT.ts` and add the following code to create your deployment module:
    ```typescript title="ignition/modules/MyNFT.ts"
    import { buildModule } from '@nomicfoundation/hardhat-ignition/modules';

    export default buildModule('MyNFTModule', (m) => {
      const initialOwner = m.getParameter('initialOwner', 'INSERT_OWNER_ADDRESS');
      const myNFT = m.contract('MyNFT', [initialOwner]);
      return { myNFT };
    });
    ```

    Replace `INSERT_OWNER_ADDRESS` with your desired owner address.

4. Deploy your contract to Polkadot Hub TestNet using the following command:

    ```bash
    npx hardhat ignition deploy ignition/modules/MyNFT.ts --network polkadotTestnet
    ```

5. Confirm the target deployment network name and chain ID when prompted:

    <div id="termynal" data-termynal markdown>
      <span data-ty="input">npx hardhat ignition deploy ignition/modules/MyNFT.ts --network polkadotHubTestnet</span>
      <span data-ty>✔ Confirm deploy to network polkadotTestnet (420420417)? … yes</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Hardhat Ignition 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deploying [ MyNFTModule ]</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Batch #1</span>
      <span data-ty> Executed MyNFTModule#MyNFT</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Batch #2</span>
      <span data-ty> Executed MyNFTModule#MyNFT.safeMint</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>[ TokenModule ] successfully deployed 🚀</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>Deployed Addresses</span>
      <span data-ty>&nbsp;</span>
      <span data-ty>MyNFTModule#MyNFT - 0x1234.......</span>
      <span data-ty="input"><span class="file-path"></span></span>
    </div>
Congratulations! You've successfully deployed an ERC-721 NFT contract to Polkadot Hub TestNet using Hardhat. Consider the following resources to build upon your progress. 

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Deploy an ERC-20__

    ---

    Walk through deploying a fully-functional ERC-20 to Polkadot Hub using Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-hardhat/)

-   <span class="badge guide">Guide</span> __Create a DApp__

    ---

    Learn step-by-step how to build a fully functional dApp that interacts with a smart contract deployed via Hardhat.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/cookbook/dapps/zero-to-hero/)

</div>


---

Page Title: Get Started with Parachain Development

- Resolved Markdown: https://docs.polkadot.com/parachains/get-started.md
- Canonical (HTML): https://docs.polkadot.com/parachains/get-started/
- Summary: Practical examples and tutorials for building and deploying Polkadot parachains, covering everything from launch to customization and cross-chain messaging.
- Word Count: 632; Token Estimate: 2346
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:73644c031ad9b4cd3591cf794ea700d71b988a11af2445fbbb7ad84d3193d837

# Get Started

The following sections provide practical recipes for building parachains on Polkadot—each focused on specific development scenarios with step-by-step, hands-on examples.

## Quick Start Guides

Quick start guides help developers set up and interact with the Polkadot parachain ecosystem using various tools and frameworks.

|                                            Tutorial                                            |         Tools         |                               Description                               |
|:----------------------------------------------------------------------------------------------:|:---------------------:|:-----------------------------------------------------------------------:|
| [Set Up the Parachain Template](/polkadot-mkdocs/parachains/launch-a-parachain/set-up-the-parachain-template/) |     Polkadot SDK      | Learn how to set up and run the Polkadot SDK Parachain Template locally |
|            [Launch a Local Parachain](/polkadot-mkdocs/parachains/testing/run-a-parachain-network/)            | Zombienet, Chopsticks |           Set up a local development environment for testing            |
|              [Fork an Existing Parachain](/polkadot-mkdocs/parachains/testing/fork-a-parachain/)               |      Chopsticks       |           Create a local fork of a live parachain for testing           |

## Launch a Simple Parachain

Learn the fundamentals of launching and deploying a parachain to the Polkadot network.

|                                            Tutorial                                            |                                Description                                |
|:----------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------:|
| [Set Up the Parachain Template](/polkadot-mkdocs/parachains/launch-a-parachain/set-up-the-parachain-template/) |                               Polkadot SDK                                |
|            [Deploy to Polkadot](/polkadot-mkdocs/parachains/launch-a-parachain/deploy-to-polkadot/)            |       Step-by-step tutorial to deploying your parachain to Polkadot       |
|               [Obtain Coretime](/polkadot-mkdocs/parachains/launch-a-parachain/obtain-coretime/)               | Learn how to acquire blockspace using Polkadot's coretime model (RegionX) |

## Customize Your Runtime

Build custom functionality for your parachain by composing and creating pallets.

|                                              Tutorial                                               |                            Description                            |
|:---------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
|     [Add Existing Pallets to the Runtime](/polkadot-mkdocs/parachains/customize-runtime/add-existing-pallets/)      |       Integrate pre-built pallets from the FRAME ecosystem        |
|      [Add Multiple Instances of a Pallet](/polkadot-mkdocs/parachains/customize-runtime/add-pallet-instances/)      |      Configure and use multiple instances of the same pallet      |
| [Add Smart Contract Functionality](/polkadot-mkdocs/parachains/customize-runtime/add-smart-contract-functionality/) | Enable smart contract capabilities using Contracts or EVM pallets |

### Pallet Development

Deep dive into creating and managing custom pallets for your parachain.

|                                             Tutorial                                              |                        Description                        |
|:-------------------------------------------------------------------------------------------------:|:---------------------------------------------------------:|
|    [Create a Custom Pallet](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/create-a-pallet/)    |       Build a pallet from scratch with custom logic       |
|        [Mock Your Runtime](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/mock-runtime/)        |       Set up a mock runtime environment for testing       |
|      [Pallet Unit Testing](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/pallet-testing/)      |      Write comprehensive tests for your pallet logic      |
| [Benchmark the Custom Pallet](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/benchmark-pallet/) | Measure and optimize pallet performance with benchmarking |

## Testing

Test your parachain in various environments before production deployment.

|                                Tutorial                                 |                       Description                       |
|:-----------------------------------------------------------------------:|:-------------------------------------------------------:|
|        [Fork a Parachain](/polkadot-mkdocs/parachains/testing/fork-a-parachain/)        |    Use Chopsticks to create a local fork for testing    |
| [Run a Parachain Network](/polkadot-mkdocs/parachains/testing/run-a-parachain-network/) | Launch a complete parachain test network with Zombienet |

## Runtime Upgrades and Maintenance

Manage your parachain's lifecycle with forkless upgrades and maintenance operations.

|                                 Tutorial                                  |                     Description                      |
|:-------------------------------------------------------------------------:|:----------------------------------------------------:|
|   [Runtime Upgrades](/polkadot-mkdocs/parachains/runtime-maintenance/runtime-upgrades/)   |   Perform forkless runtime upgrades via governance   |
| [Storage Migrations](/polkadot-mkdocs/parachains/runtime-maintenance/storage-migrations/) |  Safely migrate storage when updating runtime logic  |
|  [Unlock Parachains](/polkadot-mkdocs/parachains/runtime-maintenance/unlock-parachains/)  | Understand parachain lifecycle and unlock mechanisms |

## Interoperability

Configure your parachain for cross-chain communication using XCM (Cross-Consensus Messaging).

|                                                  Tutorial                                                  |                      Description                       |
|:----------------------------------------------------------------------------------------------------------:|:------------------------------------------------------:|
|     [Open HRMP Channels Between Parachains](/polkadot-mkdocs/parachains/interoperability/channels-between-parachains/)     | Establish communication channels with other parachains |
| [Open HRMP Channels with System Parachains](/polkadot-mkdocs/parachains/interoperability/channels-with-system-parachains/) |   Connect with Asset Hub and other system parachains   |

## Integrations

Integrate your parachain with essential ecosystem tools and services.

|                    Tutorial                    |                      Description                       |
|:----------------------------------------------:|:------------------------------------------------------:|
|  [Wallets](/polkadot-mkdocs/parachains/integrations/wallets/)  |     Integrate wallet support for user interactions     |
| [Indexers](/polkadot-mkdocs/parachains/integrations/indexers/) | Set up indexing solutions for querying blockchain data |
|  [Oracles](/polkadot-mkdocs/parachains/integrations/oracles/)  |    Connect your parachain to off-chain data sources    |

## Additional Resources

- [Polkadot SDK Documentation](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/index.html)
- [Polkadot Wiki - Parachains](https://wiki.polkadot.com/learn/learn-parachains/)


---

Page Title: Get Started with Smart Contracts

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/get-started.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/get-started/
- Summary: Practical examples for building and deploying smart contracts on Polkadot Hub, from connecting and tooling to deployment, integrations, and precompiles.
- Word Count: 964; Token Estimate: 2665
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:ba4bc97fc0e0f9d0708af65ff123f8f05f5f628b0a4a6b26c54a54262d836775

# Get Started with Smart Contracts

This resource provides quick-starts for building smart contracts on Polkadot Hub. Use the tables below to jump directly to the tools and workflows you need.

## Quick Starts

Use these curated links to get connected, get funded, and deploy your first contract.

|                     Quick Start                     |         Tools         |                           Description                           |
|:---------------------------------------------------:|:---------------------:|:---------------------------------------------------------------:|
|  [Connect to Polkadot](/polkadot-mkdocs/smart-contracts/connect/)   | Polkadot.js, MetaMask | Add the network, configure RPC, verify activity in the explorer |
|     [Get Test Tokens](/polkadot-mkdocs/smart-contracts/faucet/)     |           -           |    Request test funds to deploy and interact with contracts     |
| [Explore Transactions](/polkadot-mkdocs/smart-contracts/explorers/) | BlockScout, Routescan, Subscan | Inspect transactions, logs, token transfers, and contract state |

## Build and Test Locally

Set up local environments and CI-friendly workflows to iterate quickly and validate changes before deploying.

|                          Build and Test Locally                           |       Tools       |                  Description                   |
|:-------------------------------------------------------------------------:|:-----------------:|:----------------------------------------------:|
| [Run a Local Dev Node](/polkadot-mkdocs/smart-contracts/dev-environments/local-dev-node/) | Polkadot SDK node | Spin up a local node for iterative development |
|   [Use Remix for Development](/polkadot-mkdocs/smart-contracts/dev-environments/remix/)   |       Remix       |         Connect Remix to Polkadot Hub          |
| [Use Hardhat for Development](/polkadot-mkdocs/smart-contracts/dev-environments/hardhat/) |      Hardhat      |     Project scaffolding and configuration      |
| [Use Foundry for Development](/polkadot-mkdocs/smart-contracts/dev-environments/foundry/) |      Foundry      |     Compile, test, deploy, and verify contracts  |
| [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot) | OpenZeppelin | Generate secure ERC-20, ERC-721, and other OpenZeppelin-standard contracts for Polkadot Hub |
| [Rust for PVM](/polkadot-mkdocs/smart-contracts/for-eth-devs/dual-vm-stack/#alternative-pvm-backend) | LLMs, coding agents | Write PVM contracts in Rust; use AI assistants while tooling matures |

## Ethereum Tool Differences on Polkadot EVM

Tools like **Foundry** and **Hardhat** are built for standard Ethereum nodes. Polkadot EVM networks (such as Polkadot Hub) use the same [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/) interface, but run on a different execution environment (Substrate with REVM or PVM). As a result:

- **Local tests** (e.g., `forge test`, Hardhat's default network) run in the tool's own EVM, not Polkadot's—so behavior can differ from the real chain.
- **Time and snapshot helpers** (e.g., `evm_increaseTime`, `loadFixture`) are often not supported on Polkadot nodes.
- **Gas reports** from these tools may not match what you see on-chain.

**Recommendation:** To check chain-specific behavior, run your contracts against a [local dev node](/polkadot-mkdocs/smart-contracts/dev-environments/local-dev-node/) or a TestNet. For more details, see [EVM vs PVM](/polkadot-mkdocs/smart-contracts/for-eth-devs/evm-vs-pvm/) and [Contract Deployment](/polkadot-mkdocs/smart-contracts/for-eth-devs/contract-deployment/).

## Ethereum Developer Resources

Bridge your Ethereum knowledge with Polkadot Hub specifics: account mapping, fees, JSON-RPC, and deployment.

|                                 Ethereum Developer Guides                                 |                           Description                           |
|:-----------------------------------------------------------------------------------------:|:---------------------------------------------------------------:|
|                    [Accounts](/polkadot-mkdocs/smart-contracts/for-eth-devs/accounts/)                    | How 20‑byte Ethereum addresses map to 32‑byte Polkadot accounts |
| [Blocks, Transactions, and Fees](/polkadot-mkdocs/smart-contracts/for-eth-devs/blocks-transactions-fees/) |     Transaction types, fees, and multi‑dimensional metering     |
|                   [Gas Model](/polkadot-mkdocs/smart-contracts/for-eth-devs/gas-model/)                   |        Gas vs. weight, proof size, and storage deposits         |
|         [Contract Deployment](/polkadot-mkdocs/smart-contracts/for-eth-devs/contract-deployment/)         |     Deployment mechanics, gas estimation, and storage model     |
|               [JSON-RPC APIs](/polkadot-mkdocs/smart-contracts/for-eth-devs/json-rpc-apis/)               |        Supported Ethereum JSON-RPC methods and examples         |
|               [Dual VM Stack](/polkadot-mkdocs/smart-contracts/for-eth-devs/dual-vm-stack/)               |         Overview of EVM and native execution on the Hub         |
| [Differences: Ethereum Tools vs Polkadot EVM](/polkadot-mkdocs/smart-contracts/get-started/#ethereum-tool-differences-on-polkadot-evm) | Limitations and differences when using Foundry, Hardhat, and other tools against Polkadot nodes |

## Cookbook: Hands-on Tutorials

Follow step-by-step guides that walk through common tasks and complete dApp examples.

|                                            Tutorial                                            |        Tools        |                Description                |
|:----------------------------------------------------------------------------------------------:|:-------------------:|:-----------------------------------------:|
| [Deploy a Basic Contract](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-basic/basic-remix/) |        Remix        |      Minimal deployment walkthrough       |
|    [Deploy an ERC-20](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/)     | Remix, OpenZeppelin | Create, deploy, and mint a fungible token |
|   [Deploy an NFT (ERC-721)](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/)   | Remix, OpenZeppelin |    Build and deploy an NFT collection     |
|                 [Uniswap V2](/polkadot-mkdocs/smart-contracts/cookbook/eth-dapps/uniswap-v2/)                  |       Hardhat       | Full dApp project: compile, test, deploy  |
|               [Zero‑to‑Hero dApp](/polkadot-mkdocs/smart-contracts/cookbook/dapps/zero-to-hero/)               |      Multiple       |  End‑to‑end dApp patterns and practices   |

## Libraries

Choose the client libraries that fit your stack for connecting wallets and calling contracts.

|                      Library                       |                       Description                       |
|:--------------------------------------------------:|:-------------------------------------------------------:|
| [Ethers.js](/polkadot-mkdocs/smart-contracts/libraries/ethers-js/) | Connect, sign, and interact with contracts using Ethers |
|      [viem](/polkadot-mkdocs/smart-contracts/libraries/viem/)      |        Type‑safe EVM interactions and utilities         |
|     [Wagmi](/polkadot-mkdocs/smart-contracts/libraries/wagmi/)     |  React hooks for wallet connections and contract calls  |
|   [Web3.js](/polkadot-mkdocs/smart-contracts/libraries/web3-js/)   |             Web3 provider and contract APIs             |
|   [Web3.py](/polkadot-mkdocs/smart-contracts/libraries/web3-py/)   |  Python toolkit for on‑chain interactions and scripts   |

## Integrations

Integrate essential services like wallets, indexers, and oracles to round out your dApp.

|                    Integration                    |                Description                |
|:-------------------------------------------------:|:-----------------------------------------:|
| [Wallets](/polkadot-mkdocs/smart-contracts/integrations/wallets/) | Supported wallets and configuration notes |

## Precompiles

Discover precompiled system contracts available on the Hub and how to use them.

|                          Topic                           |                 Description                 |
|:--------------------------------------------------------:|:-------------------------------------------:|
| [Overview of Precompiles](/polkadot-mkdocs/smart-contracts/precompiles/) |  What precompiles are available on the Hub  |
|  [ETH Native](/polkadot-mkdocs/smart-contracts/precompiles/eth-native/)  |       EVM precompiles and interfaces        |
|         [XCM](/polkadot-mkdocs/smart-contracts/precompiles/xcm/)         | Cross‑chain messaging helpers for contracts |

From here, follow the quick starts to get connected, iterate locally with your preferred tools, and use the guides, libraries, integrations, and precompiles as you grow into production‑ready dApps. If you get stuck, [open an issue](https://github.com/polkadot-developers/polkadot-docs/issues/new?template=docs-issue.yml) or reach out in the community channels.


---

Page Title: Get Started with XCM

- Resolved Markdown: https://docs.polkadot.com/parachains/interoperability/get-started.md
- Canonical (HTML): https://docs.polkadot.com/parachains/interoperability/get-started/
- Summary: Unlock blockchain interoperability with XCM — Polkadot's Cross-Consensus Messaging format for cross-chain interactions.
- Word Count: 965; Token Estimate: 1472
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:40aaca9978d2098aa1140669290dca07481249954679025a9d5d1d130f3703fb

# Get Started with XCM

## Introduction

Polkadot’s unique value lies in its ability to enable interoperability between parachains and other blockchain systems. At the core of this capability is XCM (Cross-Consensus Messaging)—a flexible messaging format that facilitates communication and collaboration between independent consensus systems.

With XCM, one chain can send intents to another one, fostering a more interconnected ecosystem. Although it was developed specifically for Polkadot, XCM is a universal format, usable in any blockchain environment. This guide provides an overview of XCM’s core principles, design, and functionality, alongside practical examples of its implementation.

## Messaging Format

XCM is not a protocol but a standardized [messaging format](https://github.com/polkadot-fellows/xcm-format). It defines the structure and behavior of messages but does not handle their delivery. This separation allows developers to focus on crafting instructions for target systems without worrying about transmission mechanics.

XCM messages are intent-driven, outlining desired actions for the receiving blockchain to consider and potentially alter its state. These messages do not directly execute changes; instead, they rely on the host chain's environment to interpret and implement them. By utilizing asynchronous composability, XCM facilitates efficient execution where messages can be processed independently of their original order, similar to how RESTful services handle HTTP requests without requiring sequential processing.

## The Four Principles of XCM

XCM adheres to four guiding principles that ensure robust and reliable communication across consensus systems:

- **Asynchronous**: XCM messages operate independently of sender acknowledgment, avoiding delays due to blocked processes.
- **Absolute**: XCM messages are guaranteed to be delivered and interpreted accurately, in order, and timely. Once a message is sent, one can be sure it will be processed as intended.
- **Asymmetric**: XCM messages follow the 'fire and forget' paradigm meaning no automatic feedback is provided to the sender. Any results must be communicated separately to the sender with an additional message back to the origin.
- **Agnostic**: XCM operates independently of the specific consensus mechanisms, making it compatible across diverse systems.

These principles guarantee that XCM provides a reliable framework for cross-chain communication, even in complex environments.

## The XCM Tech Stack

![Diagram of the XCM tech stack](/polkadot-mkdocs/images/parachains/interoperability/get-started/intro-to-xcm-01.webp)

The XCM tech stack is designed to facilitate seamless interoperable communication between chains that reside within the Polkadot ecosystem. XCM can be used to express the meaning of the messages over each of the communication channels.

## Core Functionalities of XCM

XCM enhances cross-consensus communication by introducing several powerful features:

- **Programmability**: Supports dynamic message handling, allowing for more comprehensive use cases. Includes branching logic, safe dispatches for version checks, and asset operations like NFT management.
- **Functional Multichain Decomposition**: Enables mechanisms such as remote asset locking, asset namespacing, and inter-chain state referencing, with contextual message identification.
- **Bridging**: Establishes a universal reference framework for multi-hop setups, connecting disparate systems like Ethereum and Bitcoin with the Polkadot relay chain acting as a universal location.

The standardized format for messages allows parachains to handle tasks like user balances, governance, and staking, freeing the Polkadot relay chain to focus on shared security. These features make XCM indispensable for implementing scalable and interoperable blockchain applications. 

## XCM Example

The following is a simplified XCM message demonstrating a token transfer from Alice to Bob on the same chain (ParaA).

```rust
let message = Xcm(vec![
    WithdrawAsset((Here, amount).into()),
    BuyExecution { 
        fees: (Here, amount).into(), 
        weight_limit: WeightLimit::Unlimited 
    },
    DepositAsset {
        assets: All.into(),
        beneficiary: MultiLocation {
            parents: 0,
            interior: Junction::AccountId32 {
                network: None,
                id: BOB.clone().into()
            }.into(),
        }.into()
    }
]);
```

The message consists of three instructions described as follows:

- **[WithdrawAsset](https://github.com/polkadot-fellows/xcm-format?tab=readme-ov-file#withdrawasset)**: Transfers a specified number of tokens from Alice's account to a holding register.

    ```rust
        WithdrawAsset((Here, amount).into()),
    ```

    - **`Here`**: The native parachain token.
    - **`amount`**: The number of tokens that are transferred.

    The first instruction takes as an input the MultiAsset that should be withdrawn. The MultiAsset describes the native parachain token with the `Here` keyword. The `amount` parameter is the number of tokens that are transferred. The withdrawal account depends on the origin of the message. In this example the origin of the message is Alice. The `WithdrawAsset` instruction moves `amount` number of native tokens from Alice's account into the holding register.

- **[BuyExecution](https://github.com/polkadot-fellows/xcm-format?tab=readme-ov-file#buyexecution)**: Allocates fees to cover the execution [weight](/polkadot-mkdocs/reference/glossary/#weight) of the XCM instructions.

    ```rust
        BuyExecution { 
            fees: (Here, amount).into(), 
            weight_limit: WeightLimit::Unlimited 
        },
    ```

    - **`fees`**: Describes the asset in the holding register that should be used to pay for the weight.
    - **`weight_limit`**: Defines the maximum fees that can be used to buy weight.

- **[DepositAsset](https://github.com/polkadot-fellows/xcm-format?tab=readme-ov-file#depositasset)**: Moves the remaining tokens from the holding register to Bob’s account.

    ```rust
        DepositAsset {
            assets: All.into(),
            beneficiary: MultiLocation {
                parents: 0,
                interior: Junction::AccountId32 {
                    network: None,
                    id: BOB.clone().into()
                }.into(),
            }.into()
        }
    ```

    - **`All`**: The wildcard for the asset(s) to be deposited. In this case, all assets in the holding register should be deposited.
    
This step-by-step process showcases how XCM enables precise state changes within a blockchain system. You can find a complete XCM message example in the [XCM repository](https://github.com/paritytech/xcm-docs/blob/main/examples/src/0_first_look/mod.rs).

## Overview

XCM revolutionizes cross-chain communication by enabling use cases such as:

- Token transfers between blockchains.
- Asset locking for cross-chain smart contract interactions.
- Remote execution of functions on other blockchains.

These functionalities empower developers to build innovative, multi-chain applications, leveraging the strengths of various blockchain networks. To stay updated on XCM’s evolving format or contribute, visit the [XCM repository](https://github.com/paritytech/xcm-docs/blob/main/examples/src/0_first_look/mod.rs).


---

Page Title: Glossary

- Resolved Markdown: https://docs.polkadot.com/reference/glossary.md
- Canonical (HTML): https://docs.polkadot.com/reference/glossary/
- Summary: Glossary of terms used within the Polkadot ecosystem, Polkadot SDK, its subsequent libraries, and other relevant Web3 terminology.
- Word Count: 3541; Token Estimate: 5123
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:bbe75bcf42d35a441a8e98382339ac54586123233fd520c574b79dfb4006fe1b

# Glossary

Key definitions, concepts, and terminology specific to the Polkadot ecosystem are included here.

Additional glossaries from around the ecosystem you might find helpful:

- [Polkadot Wiki Glossary](https://wiki.polkadot.com/general/glossary)
- [Polkadot SDK Glossary](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/glossary/index.html)

## Authority

The role in a blockchain that can participate in consensus mechanisms. 

- **[GRANDPA](#grandpa)**: The authorities vote on chains they consider final.
- **[Blind Assignment of Blockchain Extension](#blind-assignment-of-blockchain-extension-babe) (BABE)**: The authorities are also [block authors](#block-author).

Authority sets can be used as a basis for consensus mechanisms such as the [Nominated Proof of Stake (NPoS)](#nominated-proof-of-stake-npos) protocol.

## Authority Round (Aura)

A deterministic [consensus](#consensus) protocol where block production is limited to a rotating list of [authorities](#authority) that take turns creating blocks. In authority round (Aura) consensus, most online authorities are assumed to be honest. It is often used in combination with [GRANDPA](#grandpa) as a [hybrid consensus](#hybrid-consensus) protocol.

Learn more by reading the official [Aura consensus algorithm](https://openethereum.github.io/Aura) wiki article.

## Blind Assignment of Blockchain Extension (BABE)

A [block authoring](#block-author) protocol similar to [Aura](#authority-round-aura), except [authorities](#authority) win [slots](#slot) based on a Verifiable Random Function (VRF) instead of the round-robin selection method. The winning authority can select a chain and submit a new block.

Learn more by reading the official Web3 Foundation [BABE research document](https://research.web3.foundation/Polkadot/protocols/block-production/Babe).

## Block Author

The node responsible for the creation of a block, also called _block producers_. In a Proof of Work (PoW) blockchain, these nodes are called _miners_.

## Byzantine Fault Tolerance (BFT)

The ability of a distributed computer network to remain operational if a certain proportion of its nodes or [authorities](#authority) are defective or behaving maliciously. A distributed network is typically considered Byzantine fault tolerant if it can remain functional, with up to one-third of nodes assumed to be defective, offline, actively malicious, and part of a coordinated attack.

### Byzantine Failure

The loss of a network service due to node failures that exceed the proportion of nodes required to reach consensus.

### Practical Byzantine Fault Tolerance (pBFT)

An early approach to Byzantine fault tolerance (BFT), practical Byzantine fault tolerance (pBFT) systems tolerate Byzantine behavior from up to one-third of participants.

The communication overhead for such systems is `O(n²)`, where `n` is the number of nodes (participants) in the system.

### Preimage

A preimage is the data that is input into a hash function to calculate a hash. Since a hash function is a [one-way function](https://en.wikipedia.org/wiki/One-way_function), the output, the hash, cannot be used to reveal the input, the preimage.

## Call

In the context of pallets containing functions to be dispatched to the runtime, `Call` is an enumeration data type that describes the functions that can be dispatched with one variant per pallet. A `Call` represents a [dispatch](#dispatchable) data structure object.

## Chain Specification 

A chain specification file defines the properties required to run a node in an active or new Polkadot SDK-built network. It often contains the initial genesis runtime code, network properties (such as the network's name), the initial state for some pallets, and the boot node list. The chain specification file makes it easy to use a single Polkadot SDK codebase as the foundation for multiple independently configured chains.

## Collator

An [author](#block-author) of a [parachain](#parachain) network.
They aren't [authorities](#authority) in themselves, as they require a [relay chain](#relay-chain) to coordinate [consensus](#consensus).

More details are found on the [Polkadot Collator Wiki](https://wiki.polkadot.com/learn/learn-collator/).

## Collective

Most often used to refer to an instance of the Collective pallet on Polkadot SDK-based networks such as [Kusama](#kusama) or [Polkadot](#polkadot) if the Collective pallet is part of the FRAME-based runtime for the network.

## Consensus

Consensus is the process blockchain nodes use to agree on a chain's canonical fork. It is composed of [authorship](#block-author), finality, and [fork-choice rule](#fork-choice-rulestrategy). In the Polkadot ecosystem, these three components are usually separate and the term consensus often refers specifically to authorship.

See also [hybrid consensus](#hybrid-consensus).

## Consensus Algorithm

Ensures a set of [actors](#authority)—who don't necessarily trust each other—can reach an agreement about the state as the result of some computation. Most consensus algorithms assume that up to one-third of the actors or nodes can be [Byzantine fault tolerant](#byzantine-fault-tolerance-bft).

Consensus algorithms are generally concerned with ensuring two properties:

- **Safety**: Indicating that all honest nodes eventually agreed on the state of the chain.
- **Liveness**: Indicating the ability of the chain to keep progressing.

## Consensus Engine

The node subsystem responsible for consensus tasks.

For detailed information about the consensus strategies of the [Polkadot](#polkadot) network, see the [Polkadot Consensus](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/) blog series.

See also [hybrid consensus](#hybrid-consensus).

## Coretime

The time allocated for utilizing a core, measured in relay chain blocks. There are two types of coretime: *on-demand* and *bulk*.

On-demand coretime refers to coretime acquired through bidding in near real-time for the validation of a single parachain block on one of the cores reserved specifically for on-demand orders. They are available as an on-demand coretime pool. Set of cores that are available on-demand. Cores reserved through bulk coretime could also be made available in the on-demand coretime pool, in parts or in entirety.

Bulk coretime is a fixed duration of continuous coretime represented by an NFT that can be split, shared, or resold. It is managed by the [Broker pallet](https://paritytech.github.io/polkadot-sdk/master/pallet_broker/index.html).

## Development Phrase

A [mnemonic phrase](https://en.wikipedia.org/wiki/Mnemonic#For_numerical_sequences_and_mathematical_operations) that is intentionally made public.

Well-known development accounts, such as Alice, Bob, Charlie, Dave, Eve, and Ferdie, are generated from the same secret phrase:

```
bottom drive obey lake curtain smoke basket hold race lonely fit walk
```

Many tools in the Polkadot SDK ecosystem, such as [`subkey`](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/substrate/bin/utils/subkey), allow you to implicitly specify an account using a derivation path such as `//Alice`.

## Digest

An extensible field of the [block header](#header) that encodes information needed by several actors in a blockchain network, including:

- [Light clients](#light-client) for chain synchronization.
- Consensus engines for block verification.
- The runtime itself, in the case of pre-runtime digests.

## Dispatchable

Function objects that act as the entry points in FRAME [pallets](#pallet). Internal or external entities can call them to interact with the blockchain’s state. They are a core aspect of the runtime logic, handling [transactions](#transaction) and other state-changing operations.

## Events

A means of recording that some particular [state](#state) transition happened.

In the context of [FRAME](#frame-framework-for-runtime-aggregation-of-modularized-entities), events are composable data types that each [pallet](#pallet) can individually define. Events in FRAME are implemented as a set of transient storage items inspected immediately after a block has been executed and reset during block initialization.

## Executor

A means of executing a function call in a given [runtime](#runtime) with a set of dependencies.
There are two orchestration engines in Polkadot SDK, _WebAssembly_ and _native_.

- The _native executor_ uses a natively compiled runtime embedded in the node to execute calls. This is a performance optimization available to up-to-date nodes.

- The _WebAssembly executor_ uses a [Wasm](#webassembly-wasm) binary and a Wasm interpreter to execute calls. The binary is guaranteed to be up-to-date regardless of the version of the blockchain node because it is persisted in the [state](#state) of the Polkadot SDK-based chain.

## Existential Deposit

The minimum balance an account is allowed to have in the [Balances pallet](https://paritytech.github.io/polkadot-sdk/master/pallet_balances/index.html). Accounts cannot be created with a balance less than the existential deposit amount. 

If an account balance drops below this amount, the Balances pallet uses [a FRAME System API](https://paritytech.github.io/substrate/master/frame_system/pallet/struct.Pallet.html#method.dec_ref) to drop its references to that account.

If the Balances pallet reference to an account is dropped, the account can be [reaped](https://paritytech.github.io/substrate/master/frame_system/pallet/struct.Pallet.html#method.allow_death).

## Extrinsic

A general term for data that originates outside the runtime, is included in a block, and leads to some action. This includes user-initiated transactions and inherent transactions placed into the block by the block builder.

It is a SCALE-encoded array typically consisting of a version number, signature, and varying data types indicating the resulting runtime function to be called. Extrinsics can take two forms: [inherents](#inherent-transactions) and [transactions](#transaction). 

## Fork Choice Rule/Strategy

A fork choice rule or strategy helps determine which chain is valid when reconciling several network forks. A common fork choice rule is the [longest chain](https://paritytech.github.io/polkadot-sdk/master/sc_consensus/struct.LongestChain.html), in which the chain with the most blocks is selected.

## FRAME (Framework for Runtime Aggregation of Modularized Entities)

Enables developers to create blockchain [runtime](#runtime) environments from a modular set of components called [pallets](#pallet). It utilizes a set of procedural macros to construct runtimes.

[Visit the Polkadot SDK docs for more details on FRAME.](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/frame_runtime/index.html)

## Full Node

A node that prunes historical states, keeping only recently finalized block states to reduce storage needs. Full nodes provide current chain state access and allow direct submission and validation of [extrinsics](#extrinsic), maintaining network decentralization.

## Genesis Configuration

A mechanism for specifying the initial state of a blockchain. By convention, this initial state or first block is commonly referred to as the genesis state or genesis block. The genesis configuration for Polkadot SDK-based chains is accomplished by way of a [chain specification](#chain-specification) file.

## GRANDPA

A deterministic finality mechanism for blockchains that is implemented in the [Rust](https://rust-lang.org/) programming language.

The [formal specification](https://github.com/w3f/consensus/blob/master/pdf/grandpa-old.pdf) is maintained by the [Web3 Foundation](https://web3.foundation/).

## Header

A structure that aggregates the information used to summarize a block. Primarily, it consists of cryptographic information used by [light clients](#light-client) to get minimally secure but very efficient chain synchronization.

## Hybrid Consensus

A blockchain consensus protocol that consists of independent or loosely coupled mechanisms for [block production](#block-author) and finality.

Hybrid consensus allows the chain to grow as fast as probabilistic consensus protocols, such as [Aura](#authority-round-aura), while maintaining the same level of security as deterministic finality consensus protocols, such as [GRANDPA](#grandpa).

## Inherent Transactions

A special type of unsigned transaction, referred to as _inherents_, that enables a block authoring node to insert information that doesn't require validation directly into a block.

Only the block-authoring node that calls the inherent transaction function can insert data into its block. In general, validators assume the data inserted using an inherent transaction is valid and reasonable even if it can't be deterministically verified.

## JSON-RPC

A stateless, lightweight remote procedure call protocol encoded in JavaScript Object Notation (JSON). JSON-RPC provides a standard way to call functions on a remote system by using JSON.

For Polkadot SDK, this protocol is implemented through the [Parity JSON-RPC](https://github.com/paritytech/jsonrpc) crate.

## Keystore

A subsystem for managing keys for the purpose of producing new blocks.

## Kusama

[Kusama](https://kusama.network/) is a Polkadot SDK-based blockchain that implements a design similar to the [Polkadot](#polkadot) network.

Kusama is a [canary](https://en.wiktionary.org/wiki/canary_in_a_coal_mine) network and is referred to as [Polkadot's "wild cousin."](https://wiki.polkadot.com/learn/learn-comparisons-kusama/).

As a canary network, Kusama is expected to be more stable than a test network like [Westend](#westend) but less stable than a production network like [Polkadot](#polkadot). Kusama is controlled by its network participants and is intended to be stable enough to encourage meaningful experimentation.

## libp2p

A peer-to-peer networking stack that allows the use of many transport mechanisms, including WebSockets (usable in a web browser).

Polkadot SDK uses the [Rust implementation](https://github.com/libp2p/rust-libp2p) of the `libp2p` networking stack.

## Light Client

A type of blockchain node that doesn't store the [chain state](#state) or produce blocks.

A light client can verify cryptographic primitives and provides a [remote procedure call (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call) server, enabling blockchain users to interact with the network.

## Metadata

Data that provides information about one or more aspects of a system.
The metadata that exposes information about a Polkadot SDK blockchain enables you to interact with that system.

## Nominated Proof of Stake (NPoS)

A method for determining [validators](#validator) or _[authorities](#authority)_ based on a willingness to commit their stake to the proper functioning of one or more block-producing nodes.

## Oracle

An entity that connects a blockchain to a non-blockchain data source. Oracles enable the blockchain to access and act upon information from existing data sources and incorporate data from non-blockchain systems and services.

## Origin

A [FRAME](#frame-framework-for-runtime-aggregation-of-modularized-entities) primitive that identifies the source of a [dispatched](#dispatchable) function call into the [runtime](#runtime). The FRAME System pallet defines three built-in [origins](#origin). As a [pallet](#pallet) developer, you can also define custom origins, such as those defined by the [Collective pallet](https://paritytech.github.io/substrate/master/pallet_collective/enum.RawOrigin.html).

## Pallet

A module that can be used to extend the capabilities of a [FRAME](#frame-framework-for-runtime-aggregation-of-modularized-entities)-based [runtime](#runtime).
Pallets bundle domain-specific logic with runtime primitives like [events](#events) and [storage items](#storage-item).

## Parachain

A parachain is a blockchain that derives shared infrastructure and security from a _[relay chain](#relay-chain)_.
You can learn more about parachains on the [Polkadot Wiki](https://wiki.polkadot.com/learn/learn-parachains/).

## Paseo

Paseo TestNet provisions testing on Polkadot's "production" runtime, which means less chance of feature or code mismatch when developing parachain apps. Specifically, after the [Polkadot Technical fellowship](https://wiki.polkadot.com/learn/learn-polkadot-technical-fellowship/) proposes a runtime upgrade for Polkadot, this TestNet is updated, giving a period where the TestNet will be ahead of Polkadot to allow for testing.

## Polkadot

The [Polkadot network](https://polkadot.com/) is a blockchain that serves as the central hub of a heterogeneous blockchain network. It serves the role of the [relay chain](#relay-chain) and provides shared infrastructure and security to support [parachains](#parachain).

## Polkadot Cloud

Polkadot Cloud is a platform for deploying resilient, customizable and scalable Web3 applications through Polkadot's functionality. It encompasses the wider Polkadot network infrastructure and security layer where parachains operate. The platform enables users to launch Ethereum-compatible chains, build specialized blockchains, and flexibly manage computing resources through on-demand or bulk coretime purchases. Initially launched with basic parachain functionality, Polkadot Cloud has evolved to offer enhanced flexibility with features like coretime, elastic scaling, and async backing for improved performance.

## Polkadot Hub

Polkadot Hub is a Layer 1 platform that serves as the primary entry point to the Polkadot ecosystem, providing essential functionality without requiring parachain deployment. It offers core services including smart contracts, identity management, staking, governance, and interoperability with other ecosystems, making it simple and fast for both builders and users to get started in Web3.

## PVM

The Polkadot Virtual Machine (PVM) is a custom virtual machine optimized for performance, leveraging a RISC-V-based architecture to support Solidity and any language that compiles to RISC-V. It is specifically designed for the Polkadot ecosystem, enabling smart contract deployment and execution.

## Relay Chain

Relay chains are blockchains that provide shared infrastructure and security to the [parachains](#parachain) in the network. In addition to providing [consensus](#consensus) capabilities, relay chains allow parachains to communicate and exchange digital assets without needing to trust one another.

## Rococo

A [parachain](#parachain) test network for the Polkadot network. The [Rococo](#rococo) network is a Polkadot SDK-based blockchain with an October 14, 2024 deprecation date. Development teams are encouraged to use the Paseo TestNet instead.

## Runtime

The runtime represents the [state transition function](#state-transition-function-stf) for a blockchain. In Polkadot SDK, the runtime is stored as a [Wasm](#webassembly-wasm) binary in the chain state. The Runtime is stored under a unique state key and can be modified during the execution of the state transition function.

## Slot

A fixed, equal interval of time used by consensus engines such as [Aura](#authority-round-aura) and [BABE](#blind-assignment-of-blockchain-extension-babe). In each slot, a subset of [authorities](#authority) is permitted, or obliged, to [author](#block-author) a block.

## Sovereign Account

The unique account identifier for each chain in the relay chain ecosystem. It is often used in cross-consensus (XCM) interactions to sign XCM messages sent to the relay chain or other chains in the ecosystem.

The sovereign account for each chain is a root-level account that can only be accessed using the Sudo pallet or through governance. The account identifier is calculated by concatenating the Blake2 hash of a specific text string and the registered parachain identifier.

## SS58 Address Format

A public key address based on the Bitcoin [`Base-58-check`](https://en.bitcoin.it/wiki/Base58Check_encoding) encoding. Each Polkadot SDK SS58 address uses a `base-58` encoded value to identify a specific account on a specific Polkadot SDK-based chain

The [canonical `ss58-registry`](https://github.com/paritytech/ss58-registry) provides additional details about the address format used by different Polkadot SDK-based chains, including the network prefix and website used for different networks

## State Transition Function (STF)

The logic of a blockchain that determines how the state changes when a block is processed. In Polkadot SDK, the state transition function is effectively equivalent to the [runtime](#runtime).

## Storage Item

[FRAME](#frame-framework-for-runtime-aggregation-of-modularized-entities) primitives that provide type-safe data persistence capabilities to the [runtime](#runtime).
Learn more in the [storage items](https://paritytech.github.io/polkadot-sdk/master/frame_support/storage/types/index.html) reference document in the Polkadot SDK.

## Substrate

A flexible framework for building modular, efficient, and upgradeable blockchains. Substrate is written in the [Rust](https://rust-lang.org/) programming language and is maintained by [Parity Technologies](https://www.parity.io/).

## Transaction

An [extrinsic](#extrinsic) that includes a signature that can be used to verify the account authorizing it inherently or via [signed extensions](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/signed_extensions/index.html).

## Transaction Era

A definable period expressed as a range of block numbers during which a transaction can be included in a block.
Transaction eras are used to protect against transaction replay attacks if an account is reaped and its replay-protecting nonce is reset to zero.

## Trie (Patricia Merkle Tree)

A data structure used to represent sets of key-value pairs and enables the items in the data set to be stored and retrieved using a cryptographic hash. Because incremental changes to the data set result in a new hash, retrieving data is efficient even if the data set is very large. With this data structure, you can also prove whether the data set includes any particular key-value pair without access to the entire data set.

In Polkadot SDK-based blockchains, state is stored in a trie data structure that supports the efficient creation of incremental digests. This trie is exposed to the [runtime](#runtime) as [a simple key/value map](#storage-item) where both keys and values can be arbitrary byte arrays.

## Validator

A validator is a node that participates in the consensus mechanism of the network. Its roles include block production, transaction validation, network integrity, and security maintenance.

## WebAssembly (Wasm)

An execution architecture that allows for the efficient, platform-neutral expression of
deterministic, machine-executable logic.

[Wasm](https://webassembly.org/) can be compiled from many languages, including
the [Rust](https://rust-lang.org/) programming language. Polkadot SDK-based chains use a Wasm binary to provide portable [runtimes](#runtime) that can be included as part of the chain's state.

## Weight

A convention used in Polkadot SDK-based blockchains to measure and manage the time it takes to validate a block.
Polkadot SDK defines one unit of weight as one picosecond of execution time on reference hardware.

The maximum block weight should be equivalent to one-third of the target block time with an allocation of one-third each for:

- Block construction
- Network propagation
- Import and verification

By defining weights, you can trade-off the number of transactions per second and the hardware required to maintain the target block time appropriate for your use case. Weights are defined in the runtime, meaning you can tune them using runtime updates to keep up with hardware and software improvements.

## Westend

Westend is a Parity-maintained, Polkadot SDK-based blockchain that serves as a test network for the [Polkadot](#polkadot) network.


---

Page Title: Install Polkadot SDK

- Resolved Markdown: https://docs.polkadot.com/parachains/install-polkadot-sdk.md
- Canonical (HTML): https://docs.polkadot.com/parachains/install-polkadot-sdk/
- Summary: Install all required Polkadot SDK dependencies, set up the SDK itself, and verify that it runs correctly on your machine.
- Word Count: 3240; Token Estimate: 5132
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:e918ef47de5e02071a396ea96361fba42562133a8d431f99b9fb7a8e3049bd63

# Install Polkadot SDK

This guide provides step-by-step instructions for installing the Polkadot SDK on macOS, Linux, and Windows. The installation process consists of two main parts:

- **Installing dependencies**: Setting up Rust, required system packages, and development tools.
- **Building the Polkadot SDK**: Cloning and compiling the Polkadot SDK repository.

Follow the appropriate section for your operating system to ensure all necessary tools are installed and configured properly.

## Install Dependencies: macOS

You can install Rust and set up a Substrate development environment on Apple macOS computers with Intel or Apple M1 processors.

### Before You Begin {: #before-you-begin-mac-os }

Before you install Rust and set up your development environment on macOS, verify that your computer meets the following basic requirements:

- Operating system version is 10.7 Lion or later.
- Processor speed of at least 2 GHz. Note that 3 GHz is recommended.
- Memory of at least 8 GB RAM. Note that 16 GB is recommended.
- Storage of at least 10 GB of available space.
- Broadband Internet connection.

### Install Command Line Tools

Xcode Command Line Tools provide essential build dependencies including `clang`, `make`, and other tools required to compile native crates like `librocksdb-sys`. To install them, run:

```bash
xcode-select --install
```

### Install Homebrew

In most cases, you should use Homebrew to install and manage packages on macOS computers. If you don't already have Homebrew installed on your local computer, you should download and install it before continuing.

To install Homebrew:

1. Open the Terminal application.
2. Download and install Homebrew by running the following command:

    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    ```

3. Verify Homebrew has been successfully installed by running the following command:

    ```bash
    brew --version
    ```

    The command displays output similar to the following:

    <div id="termynal" data-termynal markdown>
      <span data-ty="input"><span class="file-path"></span>brew --version</span>
      <span data-ty>Homebrew 4.3.15</span>
    </div>
### Support for Apple Silicon

Protobuf must be installed before the build process can begin. To install it, run the following command:

```bash
brew install protobuf
```

### Install Required Packages and Rust {: #install-required-packages-and-rust-mac-os }

Because the blockchain requires standard cryptography to support the generation of public/private key pairs and the validation of transaction signatures, you must also have a package that provides cryptography, such as `openssl`.

To install `openssl` and the Rust toolchain on macOS:

1. Open the Terminal application.
2. Ensure you have an updated version of Homebrew by running the following command:

    ```bash
    brew update
    ```

3. Install the `openssl` package by running the following command:

    ```bash
    brew install openssl
    ```

4. Download the `rustup` installation program and use it to install Rust by running the following command:

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```

5. Follow the prompts displayed to proceed with a default installation.
6. Update your current shell to include Cargo by running the following command:

    ```bash
    source ~/.cargo/env
    ```

7. Configure the Rust toolchain to default to the latest stable version by running the following commands:

    ```bash
    rustup default stable
    rustup update
    rustup target add wasm32-unknown-unknown
    rustup component add rust-src
    ```

8. Proceed to [Build the Polkadot SDK](#build-the-polkadot-sdk).

## Install Dependencies: Linux

Rust supports most Linux distributions. Depending on the specific distribution and version of the operating system you use, you might need to add some software dependencies to your environment. In general, your development environment should include a linker or a C-compatible compiler, such as `clang`, and an appropriate integrated development environment (IDE).

### Before You Begin {: #before-you-begin-linux }

Check the documentation for your operating system for information about the installed packages and how to download and install any additional packages you might need. For example, if you use Ubuntu, you can use the Ubuntu Advanced Packaging Tool (`apt`) to install the `build-essential` package:

```bash
sudo apt install build-essential
```

At a minimum, you need the following packages before you install Rust:

```text
clang curl git make
```

Because the blockchain requires standard cryptography to support the generation of public/private key pairs and the validation of transaction signatures, you must also have a package that provides cryptography, such as `libssl-dev` or `openssl-devel`.

### Install Required Packages and Rust {: #install-required-packages-and-rust-linux }

To install the Rust toolchain on Linux:

1. Open a terminal shell.
2. Check the packages installed on the local computer by running the appropriate package management command for your Linux distribution.
3. Add any package dependencies you are missing to your local development environment by running the appropriate package management command for your Linux distribution:

    === "Ubuntu"

        ```bash
        sudo apt install --assume-yes git clang curl libssl-dev llvm libclang-dev libudev-dev make protobuf-compiler
        ```

    === "Debian"

        ```sh
        sudo apt install --assume-yes git clang curl libssl-dev llvm libclang-dev libudev-dev make protobuf-compiler
        ```

    === "Arch"

        ```sh
        pacman -Syu --needed --noconfirm curl git clang make protobuf
        ```

    === "Fedora"

        ```sh
        sudo dnf update
        sudo dnf install clang curl git openssl-devel make protobuf-compiler
        ```

    === "OpenSUSE"

        ```sh
        sudo zypper install clang curl git openssl-devel llvm-devel libudev-devel make protobuf
        ```

    Remember that different distributions might use different package managers and bundle packages in different ways. For example, depending on your installation selections, Ubuntu Desktop and Ubuntu Server might have different packages and different requirements. However, the packages listed in the command-line examples are applicable to many common Linux distributions, including Debian, Linux Mint, MX Linux, and Elementary OS.

4. Download the `rustup` installation program and use it to install Rust by running the following command:

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```

5. Follow the prompts displayed to proceed with a default installation.
6. Update your current shell to include Cargo by running the following command:

    ```bash
    source $HOME/.cargo/env
    ```

7. Verify your installation by running the following command:

    ```bash
    rustc --version
    ```

8. Configure the Rust toolchain to default to the latest stable version by running the following commands:

    ```bash
    rustup default stable
    rustup update
    rustup target add wasm32-unknown-unknown
    rustup component add rust-src
    ```

9. Proceed to [Build the Polkadot SDK](#build-the-polkadot-sdk).

## Install Dependencies: Windows (WSL)

In general, UNIX-based operating systems—like macOS or Linux—provide a better development environment for building Substrate-based blockchains.

However, suppose your local computer uses Microsoft Windows instead of a UNIX-based operating system. In that case, you can configure it with additional software to make it a suitable development environment for building Substrate-based blockchains. To prepare a development environment on a Microsoft Windows computer, you can use Windows Subsystem for Linux (WSL) to emulate a UNIX operating environment.

### Before You Begin {: #before-you-begin-windows-wls }

Before installing on Microsoft Windows, verify the following basic requirements:

- You have a computer running a supported Microsoft Windows operating system:
    - **For Windows desktop**: You must be running Microsoft Windows 10, version 2004 or later, or Microsoft Windows 11 to install WSL.
    - **For Windows server**: You must be running Microsoft Windows Server 2019, or later, to install WSL on a server operating system.
- You have a good internet connection and access to a shell terminal on your local computer.

### Set Up Windows Subsystem for Linux

WSL enables you to emulate a Linux environment on a computer that uses the Windows operating system. The primary advantage of this approach for Substrate development is that you can use all of the code and command-line examples as described in the Substrate documentation. For example, you can run common commands—such as `ls` and `ps`—unmodified. By using WSL, you can avoid configuring a virtual machine image or a dual-boot operating system.

To prepare a development environment using WSL:

1. Check your Windows version and build number to see if WSL is enabled by default.

    If you have Microsoft Windows 10, version 2004 (Build 19041 and higher), or Microsoft Windows 11, WSL is available by default and you can continue to the next step.

    If you have an older version of Microsoft Windows installed, see the [WSL manual installation steps for older versions](https://learn.microsoft.com/en-us/windows/wsl/install-manual). If you are installing on an older version of Microsoft Windows, you can download and install WLS 2 if your computer has Windows 10, version 1903 or higher.

2. Select **Windows PowerShell** or **Command Prompt** from the **Start** menu, right-click, then **Run as administrator**.

3. In the PowerShell or Command Prompt terminal, run the following command:

    ```bash
    wsl --install
    ```

    This command enables the required WSL 2 components that are part of the Windows operating system, downloads the latest Linux kernel, and installs the Ubuntu Linux distribution by default.

    If you want to review the other Linux distributions available, run the following command:

    ```bash
    wsl --list --online
    ```

4. After the distribution is downloaded, close the terminal.

5. Click the **Start** menu, select **Shut down or sign out**, then click **Restart** to restart the computer.

    Restarting the computer is required to start the installation of the Linux distribution. It can take a few minutes for the installation to complete after you restart.

    For more information about setting up WSL as a development environment, see the [Set up a WSL development environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment) docs.

### Install Required Packages and Rust {: #install-required-packages-and-rust-windows-wls }

To install the Rust toolchain on WSL:

1. Click the **Start** menu, then select **Ubuntu**.
2. Type a UNIX user name to create a user account.
3. Type a password for your UNIX user, then retype the password to confirm it.
4. Download the latest updates for the Ubuntu distribution using the Ubuntu Advanced Packaging Tool (`apt`) by running the following command:

    ```bash
    sudo apt update
    ```

5. Add the required packages for the Ubuntu distribution by running the following command:

    ```bash
    sudo apt install --assume-yes git clang curl libssl-dev llvm libclang-dev libudev-dev make protobuf-compiler
    ```

6. Download the `rustup` installation program and use it to install Rust for the Ubuntu distribution by running the following command:

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```

7. Follow the prompts displayed to proceed with a default installation.

8. Update your current shell to include Cargo by running the following command:

    ```bash
    source ~/.cargo/env
    ```

9. Verify your installation by running the following command:

    ```bash
    rustc --version
    ```

10. Configure the Rust toolchain to use the latest stable version as the default toolchain by running the following commands:

    ```bash
    rustup default stable
    rustup update
    rustup target add wasm32-unknown-unknown
    rustup component add rust-src
    ```

11. Proceed to [Build the Polkadot SDK](#build-the-polkadot-sdk).

## Build the Polkadot SDK

After installing all dependencies, you can now clone and compile the Polkadot SDK repository to verify your setup.

### Clone the Polkadot SDK

1. Clone the Polkadot SDK repository:

    ```bash
    git clone https://github.com/paritytech/polkadot-sdk.git
    ```

2. Navigate into the project directory:

    ```bash
    cd polkadot-sdk
    ```

### Compile the Polkadot SDK

Compile the entire Polkadot SDK repository to ensure your environment is properly configured:

```bash
cargo build --release --locked
```

!!!note
    This initial compilation will take significant time, depending on your machine specifications. It compiles all components of the Polkadot SDK to verify your toolchain is correctly configured.

### Verify the Build

Once the build completes successfully, verify the installation by checking the compiled binaries:

```bash
ls target/release
```

You should see several binaries, including:

- `polkadot`: The Polkadot relay chain node.
- `polkadot-parachain`: The parachain collator node.
- `polkadot-omni-node`:The omni node for running parachains.
- `substrate-node`: The kitchensink node with many pre-configured pallets.

Verify the Polkadot binary works by checking its version:

```bash
./target/release/polkadot --version
```

This should display version information similar to:

```bash
polkadot 1.16.0-1234abcd567
```

If you see the version output without errors, your development environment is correctly configured and ready for Polkadot SDK development!

## Optional: Run the Kitchensink Node

The Polkadot SDK includes a feature-rich node called "kitchensink" located at `substrate/bin/node`. This node comes pre-configured with many pallets and features from the Polkadot SDK, making it an excellent reference for exploring capabilities and understanding how different components work together.

!!!note
    If you've already compiled the Polkadot SDK in the previous step, the `substrate-node` binary is already built and ready to use. You can skip directly to running the node.

### Run the Kitchensink Node in Development Mode

From the `polkadot-sdk` root directory, start the kitchensink node in development mode:

```bash
./target/release/substrate-node --dev
```

The `--dev` flag enables development mode, which:

- Runs a single-node development chain.
- Produces and finalizes blocks automatically.
- Uses pre-configured development accounts (Alice, Bob, etc.).
- Deletes all data when stopped, ensuring a clean state on restart.

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>./target/release/substrate-node --dev</span>
  <span data-ty>2025-10-25 09:08:53 Substrate Node</span>
  <span data-ty>2025-10-25 09:08:53 ✌️  version 3.0.0-dev-7304295748b</span>
  <span data-ty>2025-10-25 09:08:53 ❤️  by Parity Technologies &lt;admin@parity.io&gt;, 2017-2025</span>
  <span data-ty>2025-10-25 09:08:53 📋 Chain specification: Development</span>
  <span data-ty>2025-10-25 09:08:53 🏷  Node name: squalid-shelf-0944</span>
  <span data-ty>2025-10-25 09:08:53 👤 Role: AUTHORITY</span>
  <span data-ty>2025-10-25 09:08:53 💾 Database: RocksDb at /var/folders/6t/fw37yfvx14qgwv5sl5p0lcph0000gn/T/substrate0XFsh7/chains/dev/db/full</span>
  <span data-ty>2025-10-25 09:08:57 🔨 Initializing Genesis block/state (state: 0x9831…a36f, header-hash: 0x6eaf…0764)</span>
  <span data-ty>2025-10-25 09:08:57 Creating transaction pool txpool_type=ForkAware ready=Limit { count: 8192, total_bytes: 20971520 } future=Limit { count: 819, total_bytes: 2097152 }</span>
  <span data-ty>2025-10-25 09:08:57 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.</span>
  <span data-ty>2025-10-25 09:08:57 👶 Creating empty BABE epoch changes on what appears to be first startup.</span>
  <span data-ty>2025-10-25 09:08:57 Using default protocol ID "sup" because none is configured in the chain specs</span>
  <span data-ty>2025-10-25 09:08:57 Local node identity is: 12D3KooWFug81ps95nGBDXidDv8K9zth3JBwVw3nm8KanZ3Rdi4J</span>
  <span data-ty>2025-10-25 09:08:57 Running litep2p network backend</span>
  <span data-ty>2025-10-25 09:08:57 💻 Operating system: macos</span>
  <span data-ty>2025-10-25 09:08:57 💻 CPU architecture: aarch64</span>
  <span data-ty>2025-10-25 09:08:57 📦 Highest known block at #0</span>
  <span data-ty>2025-10-25 09:08:57 〽️ Prometheus exporter started at 127.0.0.1:9615</span>
  <span data-ty>2025-10-25 09:08:57 Running JSON-RPC server: addr=127.0.0.1:9944,[::1]:9944</span>
  <span data-ty>2025-10-25 09:08:57 🏁 CPU single core score: 1.55 GiBs, parallelism score: 1.43 GiBs with expected cores: 8</span>
  <span data-ty>2025-10-25 09:08:57 🏁 Memory score: 65.81 GiBs</span>
  <span data-ty>2025-10-25 09:08:57 🏁 Disk score (seq. writes): 3.20 GiBs</span>
  <span data-ty>2025-10-25 09:08:57 🏁 Disk score (rand. writes): 760.04 MiBs</span>
  <span data-ty>2025-10-25 09:08:57 👶 Starting BABE Authorship worker</span>
  <span data-ty>2025-10-25 09:08:57 Failed to load AddrCache from file, using empty instead: Failed to encode or decode AddrCache.</span>
  <span data-ty>2025-10-25 09:08:57 Loaded persisted AddrCache with 0 authority ids.</span>
  <span data-ty>2025-10-25 09:08:57 🥩 BEEFY gadget waiting for BEEFY pallet to become available...</span>
  <span data-ty>2025-10-25 09:08:57 Successfully persisted AddrCache on disk</span>
  <span data-ty>2025-10-25 09:09:00 🙌 Starting consensus session on top of parent 0x6eaf6f7689b01e21821690ce221c6f5a4eeab997242f60d232234b6c1da20764 (#0)</span>
  <span data-ty>2025-10-25 09:09:00 🎁 Prepared block for proposing at 1 (5 ms) hash: 0xf1937d6bec79950210463b701e187abe7d0d5e3eb6e089f1c15e5857966471ca; parent_hash: 0x6eaf…0764; end: NoMoreTransactions; extrinsics_count: 2</span>
  <span data-ty>2025-10-25 09:09:00 🔖 Pre-sealed block for proposal at 1. Hash now 0xcf78ec71ff34797161bfeaa325d21f14745b74cbaa59122ed879af024c7a1400, previously 0xf1937d6bec79950210463b701e187abe7d0d5e3eb6e089f1c15e5857966471ca.</span>
  <span data-ty>2025-10-25 09:09:00 👶 New epoch 0 launching at block 0xcf78…1400 (block slot 587119380 &gt;= start slot 587119380).</span>
  <span data-ty>2025-10-25 09:09:00 👶 Next epoch starts at slot 587119580</span>
  <span data-ty>2025-10-25 09:09:00 🏆 Imported #1 (0x6eaf…0764 → 0xcf78…1400)</span>
  <span data-ty>2025-10-25 09:09:00 maintain txs=(0, 0) a=1 i=1 views=[(1, 0, 0)] event=NewBestBlock { hash: 0xcf78ec71ff34797161bfeaa325d21f14745b74cbaa59122ed879af024c7a1400, tree_route: None } duration=2.034958ms</span>
  <span data-ty>2025-10-25 09:09:02 💤 Idle (0 peers), best: #1 (0xcf78…1400), finalized #0 (0x6eaf…0764), ⬇ 0 ⬆ 0</span>
  <span data-ty>2025-10-25 09:09:03 🙌 Starting consensus session on top of parent 0xcf78ec71ff34797161bfeaa325d21f14745b74cbaa59122ed879af024c7a1400 (#1)</span>
  <span data-ty>2025-10-25 09:09:03 🎁 Prepared block for proposing at 2 (4 ms) hash: 0xc5cc68c6f3dcda8bae3028f2e17a8dcd348c65750332f31af7972f25bf7a7d60; parent_hash: 0xcf78…1400; end: NoMoreTransactions; extrinsics_count: 2</span>
  <span data-ty>2025-10-25 09:09:03 🔖 Pre-sealed block for proposal at 2. Hash now 0x67d0adc440feeb28c239da9650744d3dbda899b3ca3b8dc00050ce7269ea6aee, previously 0xc5cc68c6f3dcda8bae3028f2e17a8dcd348c65750332f31af7972f25bf7a7d60.</span>
  <span data-ty>2025-10-25 09:09:03 🏆 Imported #2 (0xcf78…1400 → 0x67d0…6aee)</span>
  <span data-ty>2025-10-25 09:09:03 maintain txs=(0, 0) a=1 i=2 views=[(2, 0, 0)] event=NewBestBlock { hash: 0x67d0adc440feeb28c239da9650744d3dbda899b3ca3b8dc00050ce7269ea6aee, tree_route: None } duration=301µs</span>
  <span data-ty>2025-10-25 09:09:06 🙌 Starting consensus session on top of parent 0x67d0adc440feeb28c239da9650744d3dbda899b3ca3b8dc00050ce7269ea6aee (#2)</span>
  <span data-ty>2025-10-25 09:09:06 🎁 Prepared block for proposing at 3 (5 ms) hash: 0x29eaa5f82783284b25e9c81e851ae5be63c54de28c94f438cbd733f680172ea2; parent_hash: 0x67d0…6aee; end: NoMoreTransactions; extrinsics_count: 2</span>
  <span data-ty>2025-10-25 09:09:06 🔖 Pre-sealed block for proposal at 3. Hash now 0x8fb26c9192f8738b8971107072f9d106d364ee7f1ece94d8ae5acc9f69cc522f, previously 0x29eaa5f82783284b25e9c81e851ae5be63c54de28c94f438cbd733f680172ea2.</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
You should see log output indicating the node is running and producing blocks, with increasing block numbers after `finalized`.

### Interact with the Kitchensink Node

The kitchensink node is accessible at `ws://localhost:9944`. Open [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) in your browser to explore its features and connect to the local node.

1. Click the network icon in the top left corner.
2. Scroll to **Development** and select **Local Node**.
3. Click **Switch** to connect to your local node.

![](/polkadot-mkdocs/images/parachains/install-polkadot-sdk/install-polkadot-sdk-1.webp)

Once connected, the interface updates its color scheme to indicate a successful connection to the local node.

![](/polkadot-mkdocs/images/parachains/install-polkadot-sdk/install-polkadot-sdk-2.webp)

You can now explore the various pallets and features included in the kitchensink node, making it a valuable reference as you develop your own blockchain applications.

To stop the node, press `Control-C` in the terminal.

## Where to Go Next

<div class="grid cards" markdown>

-   __Get Started with Parachain Development__

    ---

    Practical examples and tutorials for building and deploying Polkadot parachains, covering everything from launch to customization and cross-chain messaging.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/get-started/)
 
</div>


---

Page Title: Interoperability

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/interoperability.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/interoperability/
- Summary: Explore the importance of interoperability in the Polkadot ecosystem, covering XCM, bridges, and cross-chain communication.
- Word Count: 585; Token Estimate: 771
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:8aa33264653787e25ff7726b96faee18b4b33f8015dae9253b21da11c0b92fad

# Interoperability

## Introduction

Interoperability lies at the heart of the Polkadot ecosystem, enabling communication and collaboration across a diverse range of blockchains. By bridging the gaps between parachains, relay chains, and even external networks, Polkadot unlocks the potential for truly decentralized applications, efficient resource sharing, and scalable solutions.

Polkadot’s design ensures that blockchains can transcend their individual limitations by working together as part of a unified system. This cooperative architecture is what sets Polkadot apart in the blockchain landscape.

## Why Interoperability Matters

The blockchain ecosystem is inherently fragmented. Different blockchains excel in specialized domains such as finance, gaming, or supply chain management, but these chains function in isolation without interoperability. This lack of connectivity stifles the broader utility of blockchain technology.

Interoperability solves this problem by enabling blockchains to:

- **Collaborate across networks**: Chains can interact to share assets, functionality, and data, creating synergies that amplify their individual strengths.
- **Achieve greater scalability**: Specialized chains can offload tasks to others, optimizing performance and resource utilization.
- **Expand use-case potential**: Cross-chain applications can leverage features from multiple blockchains, unlocking novel user experiences and solutions.

In the Polkadot ecosystem, interoperability transforms a collection of isolated chains into a cohesive, efficient network, pushing the boundaries of what blockchains can achieve together.

## Key Mechanisms for Interoperability

At the core of Polkadot's cross-chain collaboration are foundational technologies designed to break down barriers between networks. These mechanisms empower blockchains to communicate, share resources, and operate as a cohesive ecosystem.

### Cross-Consensus Messaging (XCM): The Backbone of Communication

Polkadot's Cross-Consensus Messaging (XCM) is the standard framework for interaction between parachains, relay chains, and, eventually, external blockchains. XCM provides a trustless, secure messaging format for exchanging assets, sharing data, and executing cross-chain operations.

Through XCM, decentralized applications can:

- Transfer tokens and other assets across chains.
- Coordinate complex workflows that span multiple blockchains.
- Enable seamless user experiences where underlying blockchain differences are invisible.
- XCM exemplifies Polkadot’s commitment to creating a robust and interoperable ecosystem.

For further information about XCM, check the [Get Started with XCM](/polkadot-mkdocs/parachains/interoperability/get-started/) article.

### Bridges: Connecting External Networks

While XCM enables interoperability within the Polkadot ecosystem, bridges extend this functionality to external blockchains such as Ethereum and Bitcoin. By connecting these networks, bridges allow Polkadot-based chains to access external liquidity, additional functionalities, and broader user bases.

With bridges, developers and users gain the ability to:

- Integrate external assets into Polkadot-based applications.
- Combine the strengths of Polkadot’s scalability with the liquidity of other networks.
- Facilitate accurate multi-chain applications that transcend ecosystem boundaries.

For more information about bridges in the Polkadot ecosystem, see the [Bridge Hub](/polkadot-mkdocs/reference/polkadot-hub/bridging/) guide.

## The Polkadot Advantage

Polkadot was purpose-built for interoperability. Unlike networks that add interoperability as an afterthought, Polkadot integrates it as a fundamental design principle. This approach offers several distinct advantages:

- **Developer empowerment**: Polkadot’s interoperability tools allow developers to build applications that leverage multiple chains’ capabilities without added complexity.
- **Enhanced ecosystem collaboration**: Chains in Polkadot can focus on their unique strengths while contributing to the ecosystem’s overall growth.
- **Future-proofing blockchain**: By enabling seamless communication, Polkadot ensures its ecosystem can adapt to evolving demands and technologies.

## Looking Ahead

Polkadot’s vision of interoperability extends beyond technical functionality, representing a shift towards a more collaborative blockchain landscape. By enabling chains to work together, Polkadot fosters innovation, efficiency, and accessibility, paving the way for a decentralized future where blockchains are not isolated competitors but interconnected collaborators.


---

Page Title: JSON-RPC APIs

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/for-eth-devs/json-rpc-apis.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/for-eth-devs/json-rpc-apis/
- Summary: JSON-RPC APIs guide for Polkadot Hub, covering supported methods, parameters, and examples for interacting with the chain.
- Word Count: 4012; Token Estimate: 9712
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:f07175cbe34280c5145ffeabf00bff70df102d7435c9d0c07700f8eb19cd69bc

# JSON-RPC APIs

## Introduction

Polkadot Hub provides Ethereum compatibility through its JSON-RPC interface, allowing developers to interact with the chain using familiar Ethereum tooling and methods. This document outlines the supported [Ethereum JSON-RPC methods](https://ethereum.org/developers/docs/apis/json-rpc/#json-rpc-methods) and provides examples of how to use them.

This guide uses the Polkadot Hub TestNet endpoint:

```text
https://services.polkadothub-rpc.com/testnet
```

## Available Methods

### eth_accounts

Returns a list of addresses owned by the client. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_accounts).

**Parameters**:

None.

**Example**:

```bash title="eth_accounts"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_accounts",
    "params":[],
    "id":1
}'
```

---

### eth_blockNumber

Returns the number of the most recent block. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_blocknumber).

**Parameters**:

None.

**Example**:

```bash title="eth_blockNumber"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_blockNumber",
    "params":[],
    "id":1
}'
```

---

### eth_call

Executes a new message call immediately without creating a transaction. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_call).

**Parameters**:

- **`transaction` ++"object"++**: The transaction call object.
    - **`to` ++"string"++**: Recipient address of the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`data` ++"string"++**: Hash of the method signature and encoded parameters. Must be a [data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`from` ++"string"++**: (Optional) Sender's address for the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`gas` ++"string"++**: (Optional) Gas limit to execute the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`gasPrice` ++"string"++**: (Optional) Gas price per unit of gas. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`value` ++"string"++**: (Optional) Value in wei to send with the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
- **`blockValue` ++"string"++**: (Optional) Block tag or block number to execute the call at. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
- **`stateOverrides` ++"object"++**: (Optional) A mapping of addresses to state overrides. Allows temporary modification of account state for the duration of the call without persisting changes on-chain. Conforms to the [Geth state override specification](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-eth#state-override-set).
    - **Key**: Account address as a hex string (e.g., `"0x1234..."`)
    - **Value**: An object with the following optional fields:
        - **`balance` ++"string"++**: (Optional) Fake balance to set for the account. Must be a quantity string.
        - **`nonce` ++"string"++**: (Optional) Fake nonce to set for the account. Must be a quantity string.
        - **`code` ++"string"++**: (Optional) Fake bytecode to inject at the account address. On Polkadot Hub, this field accepts both EVM bytecode and PolkaVM (PVM) bytecode, which is detected automatically via magic bytes.
        - **`state` ++"object"++**: (Optional) Completely replaces all storage slots for the account. A mapping of storage keys to storage values (both as 32-byte hex strings). Unspecified slots are zeroed. Mutually exclusive with `stateDiff`.
        - **`stateDiff` ++"object"++**: (Optional) Patches only the specified storage slots, leaving all other slots unchanged. A mapping of storage keys to storage values (both as 32-byte hex strings). Mutually exclusive with `state`.
        - **`movePrecompileToAddress` ++"string"++**: (Optional) Accepted for Geth compatibility but has no effect, as Polkadot Hub does not use relocatable precompile addresses.

**Example**:

=== "Basic"

    ```bash
    curl -X POST https://services.polkadothub-rpc.com/testnet \
    -H "Content-Type: application/json" \
    --data '{
        "jsonrpc":"2.0",
        "method":"eth_call",
        "params":[{
            "to": "INSERT_RECIPIENT_ADDRESS",
            "data": "INSERT_ENCODED_CALL"
        }, "INSERT_BLOCK_VALUE"],
        "id":1
    }'
    ```

    Ensure to replace `INSERT_RECIPIENT_ADDRESS`, `INSERT_ENCODED_CALL`, and `INSERT_BLOCK_VALUE` with the appropriate values.

=== "With state overrides"

    ```bash
    curl -X POST https://services.polkadothub-rpc.com/testnet \
    -H "Content-Type: application/json" \
    --data '{
        "jsonrpc":"2.0",
        "method":"eth_call",
        "params":[{
            "to": "INSERT_RECIPIENT_ADDRESS",
            "data": "INSERT_ENCODED_CALL"
        }, "latest", {
            "INSERT_ADDRESS": {
                "balance": "0xDE0B6B3A7640000"
            }
        }],
        "id":1
    }'
    ```

    In this example, the account at `INSERT_ADDRESS` is temporarily assigned a balance of 1 native token (10^18 in its smallest denomination) for the duration of the call. Ensure to replace `INSERT_RECIPIENT_ADDRESS`, `INSERT_ENCODED_CALL`, and `INSERT_ADDRESS` with the appropriate values.

!!! note "Differences from Ethereum"
    Polkadot Hub's `eth_call` state overrides are fully compatible with the [Geth state override specification](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-eth#state-override-set). One Polkadot-specific extension is that the `code` field accepts both EVM bytecode and PolkaVM (PVM) bytecode, detected automatically via magic bytes at the start of the blob.

---

### eth_chainId

Returns the chain ID used for signing transactions. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_chainid).

**Parameters**:

None.

**Example**:

```bash title="eth_chainId"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_chainId",
    "params":[],
    "id":1
}'
```

---

### eth_estimateGas

Estimates gas required for a transaction. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_estimategas).

**Parameters**:

- **`transaction` ++"object"++**: The transaction call object.
    - **`to` ++"string"++**: Recipient address of the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`data` ++"string"++**: Hash of the method signature and encoded parameters. Must be a [data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`from` ++"string"++**: (Optional) Sender's address for the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`gas` ++"string"++**: (Optional) Gas limit to execute the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`gasPrice` ++"string"++**: (Optional) Gas price per unit of gas. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`value` ++"string"++**: (Optional) Value in wei to send with the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
- **`blockValue` ++"string"++**: (Optional) Block tag or block number to execute the call at. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_estimateGas"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_estimateGas",
    "params":[{
        "to": "INSERT_RECIPIENT_ADDRESS",
        "data": "INSERT_ENCODED_FUNCTION_CALL"
    }],
    "id":1
}'
```

Ensure to replace the `INSERT_RECIPIENT_ADDRESS` and `INSERT_ENCODED_CALL` with the proper values.

---

### eth_gasPrice

Returns the current gas price in Wei. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gasprice).

**Parameters**:

None.

**Example**:

```bash title="eth_gasPrice"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_gasPrice",
    "params":[],
    "id":1
}'
```

---

### eth_getBalance

Returns the balance of a given address. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getbalance).

**Parameters**:

- **`address` ++"string"++**: Address to query balance. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`blockValue` ++"string"++**: (Optional) The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_getBalance"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getBalance",
    "params":["INSERT_ADDRESS", "INSERT_BLOCK_VALUE"],
    "id":1
}'
```

Ensure to replace the `INSERT_ADDRESS` and `INSERT_BLOCK_VALUE` with the proper values.

---

### eth_getBlockByHash

Returns information about a block by its hash. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblockbyhash).

**Parameters**:

- **`blockHash` ++"string"++**: The hash of the block to retrieve. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`fullTransactions` ++"boolean"++**: If `true`, returns full transaction details; if `false`, returns only transaction hashes.

**Example**:

```bash title="eth_getBlockByHash"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getBlockByHash",
    "params":["INSERT_BLOCK_HASH", INSERT_BOOLEAN],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_HASH` and `INSERT_BOOLEAN` with the proper values.

---

### eth_getBlockByNumber

Returns information about a block by its number. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblockbynumber).

**Parameters**:

- **`blockValue` ++"string"++**: (Optional) The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
- **`fullTransactions` ++"boolean"++**: If `true`, returns full transaction details; if `false`, returns only transaction hashes.

**Example**:

```bash title="eth_getBlockByNumber"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getBlockByNumber",
    "params":["INSERT_BLOCK_VALUE", INSERT_BOOLEAN],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_VALUE` and `INSERT_BOOLEAN` with the proper values.

---

### eth_getBlockTransactionCountByNumber

Returns the number of transactions in a block from a block number. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblocktransactioncountbynumber).

**Parameters**:

- **`blockValue` ++"string"++**: The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_getBlockTransactionCountByNumber"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getBlockTransactionCountByNumber",
    "params":["INSERT_BLOCK_VALUE"],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_VALUE` with the proper values.

---

### eth_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block hash. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblocktransactioncountbyhash).

**Parameters**:

- **`blockHash` ++"string"++**: The hash of the block to retrieve. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.

**Example**:

```bash title="eth_getBlockTransactionCountByHash"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getBlockTransactionCountByHash",
    "params":["INSERT_BLOCK_HASH"],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_HASH` with the proper values.

---

### eth_getCode

Returns the code at a given address. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getcode).

**Parameters**:

- **`address` ++"string"++**: Contract or account address to query code. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`blockValue` ++"string"++**: (Optional) The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_getCode"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getCode",
    "params":["INSERT_ADDRESS", "INSERT_BLOCK_VALUE"],
    "id":1
}'
```

Ensure to replace the `INSERT_ADDRESS` and `INSERT_BLOCK_VALUE` with the proper values.

---

### eth_getLogs

Returns an array of all logs matching a given filter object. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getlogs).

**Parameters**:

- **`filter` ++"object"++**: The filter object.
    - **`fromBlock` ++"string"++**: (Optional) Block number or tag to start from. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
    - **`toBlock` ++"string"++**: (Optional) Block number or tag to end at. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
    - **`address` ++"string" or "array of strings"++**: (Optional) Contract address or a list of addresses from which to get logs. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`topics` ++"array of strings"++**: (Optional) Array of topics for filtering logs. Each topic can be a single [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string or an array of such strings (meaning OR).
    - **`blockhash` ++"string"++**: (Optional) Hash of a specific block. Cannot be used with `fromBlock` or `toBlock`. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.

**Example**:

```bash title="eth_getLogs"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getLogs",
    "params":[{
        "fromBlock": "latest",
        "toBlock": "latest"
    }],
    "id":1
}'
```

---

### eth_getStorageAt

Returns the value from a storage position at a given address. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getstorageat).

**Parameters**:

- **`address` ++"string"++**: Contract or account address to query code. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`storageKey` ++"string"++**: Position in storage to retrieve data from. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
- **`blockValue` ++"string"++**: (Optional) The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_getStorageAt"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getStorageAt",
    "params":["INSERT_ADDRESS", "INSERT_STORAGE_KEY", "INSERT_BLOCK_VALUE"],
    "id":1
}'
```

Ensure to replace the `INSERT_ADDRESS`, `INSERT_STORAGE_KEY`, and `INSERT_BLOCK_VALUE` with the proper values.

---

### eth_getTransactionCount

Returns the number of transactions sent from an address (nonce). [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gettransactioncount).

**Parameters**:

- **`address` ++"string"++**: Address to query balance. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`blockValue` ++"string"++**: (Optional) The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).

**Example**:

```bash title="eth_getTransactionCount"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getTransactionCount",
    "params":["INSERT_ADDRESS", "INSERT_BLOCK_VALUE"],
    "id":1
}'
```

Ensure to replace the `INSERT_ADDRESS` and `INSERT_BLOCK_VALUE` with the proper values.

---

### eth_getTransactionByHash

Returns information about a transaction by its hash. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gettransactionbyhash).

**Parameters**:

- **`transactionHash` ++"string"++**: The hash of the transaction. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.

**Example**:

```bash title="eth_getTransactionByHash"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getTransactionByHash",
    "params":["INSERT_TRANSACTION_HASH"],
    "id":1
}'
```

Ensure to replace the `INSERT_TRANSACTION_HASH` with the proper values.

---

### eth_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gettransactionbyblocknumberandindex).

**Parameters**:

- **`blockValue` ++"string"++**: The block value to be fetched. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
- **`transactionIndex` ++"string"++**: The index of the transaction in the block. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.

**Example**:

```bash title="eth_getTransactionByBlockNumberAndIndex"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getTransactionByBlockNumberAndIndex",
    "params":["INSERT_BLOCK_VALUE", "INSERT_TRANSACTION_INDEX"],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_VALUE` and `INSERT_TRANSACTION_INDEX` with the proper values.

---

### eth_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gettransactionbyblockhashandindex).

**Parameters**:

- **`blockHash` ++"string"++**: The hash of the block. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`transactionIndex` ++"string"++**: The index of the transaction in the block. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.

**Example**:

```bash title="eth_getTransactionByBlockHashAndIndex"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getTransactionByBlockHashAndIndex",
    "params":["INSERT_BLOCK_HASH", "INSERT_TRANSACTION_INDEX"],
    "id":1
}'
```

Ensure to replace the `INSERT_BLOCK_HASH` and `INSERT_TRANSACTION_INDEX` with the proper values.

---

### eth_getTransactionReceipt

Returns the receipt of a transaction by transaction hash. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_gettransactionreceipt).

**Parameters**:

- **`transactionHash` ++"string"++**: The hash of the transaction. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.

**Example**:

```bash title="eth_getTransactionReceipt"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_getTransactionReceipt",
    "params":["INSERT_TRANSACTION_HASH"],
    "id":1
}'
```

Ensure to replace the `INSERT_TRANSACTION_HASH` with the proper values.

---

### eth_maxPriorityFeePerGas

Returns an estimate of the current priority fee per gas, in Wei, to be included in a block.

**Parameters**:

None.

**Example**:

```bash title="eth_maxPriorityFeePerGas"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_maxPriorityFeePerGas",
    "params":[],
    "id":1
}'
```

---

### eth_sendRawTransaction

Submits a raw transaction. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_sendrawtransaction).

**Parameters**:

- **`callData` ++"string"++**: Signed transaction data. Must be a [data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.

**Example**:

```bash title="eth_sendRawTransaction"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_sendRawTransaction",
    "params":["INSERT_CALL_DATA"],
    "id":1
}'
```

Ensure to replace the `INSERT_CALL_DATA` with the proper values.

---

### eth_sendTransaction

Creates and sends a new transaction. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_sendtransaction).

**Parameters**:

- **`transaction` ++"object"++**: The transaction object.
    - **`from` ++"string"++**: Address sending the transaction. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`to` ++"string"++**: (Optional) Recipient address. No need to provide this value when deploying a contract. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`gas` ++"string"++**: (optional, default: `90000`) gas limit for execution. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`gasPrice` ++"string"++**: (Optional) Gas price per unit. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`value` ++"string"++**: (Optional) Amount of Ether to send. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`data` ++"string"++**: (Optional) Contract bytecode or encoded method call. Must be a [data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`nonce` ++"string"++**: (Optional) Transaction nonce. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.

**Example**:

```bash title="eth_sendTransaction"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_sendTransaction",
    "params":[{
        "from": "INSERT_SENDER_ADDRESS",
        "to": "INSERT_RECIPIENT_ADDRESS",
        "gas": "INSERT_GAS_LIMIT",
        "gasPrice": "INSERT_GAS_PRICE",
        "value": "INSERT_VALUE",
        "input": "INSERT_INPUT_DATA",
        "nonce": "INSERT_NONCE"
    }],
    "id":1
}'
```

Ensure to replace the `INSERT_SENDER_ADDRESS`, `INSERT_RECIPIENT_ADDRESS`, `INSERT_GAS_LIMIT`, `INSERT_GAS_PRICE`, `INSERT_VALUE`, `INSERT_INPUT_DATA`, and `INSERT_NONCE` with the proper values.

---

### eth_syncing

Returns an object with syncing data or `false` if not syncing. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#eth_syncing).

**Parameters**:

None.

**Example**:

```bash title="eth_syncing"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"eth_syncing",
    "params":[],
    "id":1
}'
```

---

### net_listening

Returns `true` if the client is actively listening for network connections, otherwise `false`. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#net_listening).

**Parameters**:

None.

**Example**:

```bash title="net_listening"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"net_listening",
    "params":[],
    "id":1
}'
```

---

### net_peerCount

Returns the number of peers connected to the client.

**Parameters**:

None.

**Example**:

```bash title="net_peerCount"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"net_peerCount",
    "params":[],
    "id":1
}'
```

---

### net_version

Returns the current network ID as a string. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#net_version).

**Parameters**:

None.

**Example**:

```bash title="net_version"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"net_version",
    "params":[],
    "id":1
}'
```

---

### system_health

Returns information about the health of the system.

**Parameters**:

None.

**Example**:

```bash title="system_health"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"system_health",
    "params":[],
    "id":1
}'
```

---

### web3_clientVersion

Returns the current client version. [Reference](https://ethereum.org/developers/docs/apis/json-rpc/#web3_clientversion).

**Parameters**:

None.

**Example**:

```bash title="web3_clientVersion"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"web3_clientVersion",
    "params":[],
    "id":1
}'
```

---

### debug_traceBlockByNumber 

Traces a block's execution by its number and returns a detailed execution trace for each transaction.

**Parameters**:

- **`blockValue` ++"string"++**: The block number or tag to trace. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
- **`options` ++"object"++**: (Optional) An object containing tracer options.
    - **`tracer` ++"string"++**: The name of the tracer to use (e.g., `"callTracer"`, `"opTracer"`).
    - Other tracer-specific options may be supported.

**Example**:

```bash title="debug_traceBlockByNumber"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"debug_traceBlockByNumber",
    "params":["INSERT_BLOCK_VALUE", {"tracer": "callTracer"}],
    "id":1
}'
```

Ensure to replace `INSERT_BLOCK_VALUE` with a proper block number if needed.

---

### debug_traceTransaction

Traces the execution of a single transaction by its hash and returns a detailed execution trace.

**Parameters**:

- **`transactionHash` ++"string"++**: The hash of the transaction to trace. Must be a [32 byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
- **`options` ++"object"++**: (Optional) An object containing tracer options (e.g., `tracer: "callTracer"`).

**Example**:

```bash title="debug_traceTransaction"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"debug_traceTransaction",
    "params":["INSERT_TRANSACTION_HASH", {"tracer": "callTracer"}],
    "id":1
}'
```

Ensure to replace the `INSERT_TRANSACTION_HASH` with the proper value.

!!! note "Differences from Ethereum (Geth)"
    When using the default struct logger (opcode tracer), there is a difference in how `gasCost` is calculated for CALL-like opcodes (`CALL`, `DELEGATECALL`, `STATICCALL`, `CREATE`, `CREATE2`):

    - **Geth behavior**: The `gasCost` includes the opcode's intrinsic cost plus all gas forwarded to child calls.
    - **Polkadot Hub behavior**: The `gasCost` includes only the opcode's intrinsic cost, excluding forwarded gas. The intrinsic cost covers:
        - Base cost of the CALL opcode.
        - Post-call costs (for example, copying return data back to the caller's memory).

---

### debug_traceCall

Executes a new message call and returns a detailed execution trace without creating a transaction on the blockchain.

**Parameters**:

- **`transaction` ++"object"++**: The transaction call object, similar to `eth_call` parameters.
    - **`to` ++"string"++**: Recipient address of the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`data` ++"string"++**: Hash of the method signature and encoded parameters. Must be a [data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`from` ++"string"++**: (Optional) Sender's address for the call. Must be a [20-byte data](https://ethereum.org/developers/docs/apis/json-rpc/#unformatted-data-encoding) string.
    - **`gas` ++"string"++**: (Optional) Gas limit to execute the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`gasPrice` ++"string"++**: (Optional) Gas price per unit of gas. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
    - **`value` ++"string"++**: (Optional) Value in wei to send with the call. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string.
- **`blockValue` ++"string"++**: (Optional) Block tag or block number to execute the call at. Must be a [quantity](https://ethereum.org/developers/docs/apis/json-rpc/#quantities-encoding) string or a [default block parameter](https://ethereum.org/developers/docs/apis/json-rpc/#default-block).
- **`options` ++"object"++**: (Optional) An object containing tracer options (e.g., `tracer: "callTracer"`).

**Example**:

```bash title="debug_traceCall"
curl -X POST https://services.polkadothub-rpc.com/testnet \
-H "Content-Type: application/json" \
--data '{
    "jsonrpc":"2.0",
    "method":"debug_traceCall",
    "params":[{
        "from": "INSERT_SENDER_ADDRESS",
        "to": "INSERT_RECIPIENT_ADDRESS",
        "data": "INSERT_ENCODED_CALL"
    }, "INSERT_BLOCK_VALUE", {"tracer": "callTracer"}],
    "id":1
}'
```

Ensure to replace the `INSERT_SENDER_ADDRESS`, `INSERT_RECIPIENT_ADDRESS`, `INSERT_ENCODED_CALL`, and `INSERT_BLOCK_VALUE` with the proper value.

---

## Response Format

All responses follow the standard JSON-RPC 2.0 format:

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": ... // The return value varies by method
}
```

## Error Handling

If an error occurs, the response will include an error object:

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "error": {
        "code": -32000,
        "message": "Error message here"
    }
}
```


---

Page Title: Networks

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/networks.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/networks/
- Summary: Explore Polkadot's networks, with Paseo as the official Polkadot TestNet for parachain and dApp development, plus Kusama and Westend for specialized use cases.
- Word Count: 1497; Token Estimate: 2025
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:3c5781d367df932ca11255253b6ff946a269caf3e3b2b29fc3cea524d3636606

# Networks

## Introduction

The Polkadot ecosystem is built on a robust set of networks that enable secure, scalable development. Whether you are testing new features or deploying to live production, Polkadot offers several network layers tailored to each stage of the development process. From local environments to the official Polkadot TestNet (Paseo), developers can thoroughly test, iterate, and validate their applications before deploying to Polkadot MainNet. This guide will introduce you to Polkadot's various networks and explain how they fit into the development workflow.

## Network Overview 

Polkadot's development process is structured to ensure that new features and upgrades are rigorously tested before deployment to live production networks. For all parachain and dApp developers, the recommended progression follows a well-defined path: starting from local environments, testing on Paseo (the official Polkadot TestNet), and ultimately deploying to either Kusama or Polkadot MainNet. The diagram below outlines the recommended development flow:

``` mermaid
flowchart LR
    id1[Local] --> id2[Paseo TestNet] --> id3[Kusama]
    id1[Local] --> id2[Paseo TestNet] --> id4[Polkadot MainNet]
```

This flow ensures developers can thoroughly test and iterate without risking real tokens or affecting production networks. **Paseo TestNet is the recommended testing environment for all deployments**, whether targeting Kusama or Polkadot. Testing tools like [Chopsticks](#chopsticks) make it easier to experiment safely before releasing to production.

### Recommended Development Path

For all parachain teams and dApp developers, the recommended journey looks like this:

1. **Local development node**: Development starts in a local environment, where developers can create, test, and iterate on upgrades or new features using a local development node. This stage enables rapid experimentation in an isolated setup with no external dependencies. Parachain developers can leverage local TestNets, such as [Zombienet](#zombienet), for multi-chain testing.

2. **Paseo (Polkadot TestNet)**: After testing locally, deploy to [Paseo](#polkadot-testnet-paseo), the official Polkadot TestNet. Paseo is a stable, community-run TestNet that mirrors Polkadot's runtime and is specifically designed for parachain teams and dApp developers to test before deploying to production networks. **This is the recommended TestNet whether you plan to deploy to Kusama or Polkadot MainNet**.

3. **Production deployment**: After thorough testing on Paseo, features are considered ready for deployment to either:
   - **Kusama**: An experimental "canary" network with real economic value, suitable for teams that want to deploy in a faster-moving production environment before Polkadot
   - **Polkadot MainNet**: The primary production network with enterprise-grade security

### Alternative Networks

The Polkadot ecosystem also includes alternative networks, but these are not part of the recommended development flow:

- **Westend**: A protocol-focused TestNet maintained by Parity Technologies, primarily used for testing low-level protocol changes and infrastructure updates. Westend is designed for infrastructure operators and core protocol developers, not for general parachain or dApp development. **The onboarding process for Westend is not clearly documented, and external developers should use Paseo TestNet instead.** See the [Other Networks](#other-networks) section for details.

!!!note
    The Rococo TestNet was deprecated on October 14, 2024. Paseo is now the official Polkadot TestNet for parachain and dApp development. For protocol-level testing, teams may use Westend, but most external developers should use Paseo as the default TestNet.

## Polkadot MainNet

Polkadot is the production network where real value and live applications operate. After thorough testing on Paseo and local development environments, teams deploy their parachains and dApps to Polkadot MainNet. Polkadot provides enterprise-grade security through its shared security model, where all parachains benefit from the collective security of the relay chain's validator set.

The native token for Polkadot is DOT. For more information about DOT, visit the [Native Assets](https://wiki.polkadot.com/learn/learn-dot/) page on the Polkadot Wiki.

## Polkadot TestNet (Paseo)

[Paseo](https://github.com/paseo-network) is the official Polkadot TestNet for parachain teams and dApp developers. As a stable, community-run TestNet that mirrors Polkadot's runtime, Paseo is specifically designed to provide a reliable testing environment for teams preparing to deploy to Polkadot MainNet.

**Paseo is the recommended TestNet for the vast majority of external developers.** It provides a Polkadot-like environment without the risks and costs associated with live networks, making it ideal for testing parachains, smart contracts, cross-chain messaging (XCM), governance mechanisms, and other application features.

Key characteristics of Paseo:

- **Official Polkadot TestNet**: Recognized as the primary TestNet for Polkadot ecosystem development
- **Stable and reliable**: Maintained by the community with a focus on stability and uptime
- **Runtime parity**: Mirrors Polkadot's runtime, ensuring your tests accurately reflect MainNet behavior
- **Community-driven**: Governed and operated by Polkadot community members
- **Purpose-built for developers**: Specifically designed for parachain and dApp testing workflows

The native token for Paseo is PAS. TestNet tokens are available from the [Polkadot faucet](https://faucet.polkadot.io/). Additional information on PAS is available on the [Native Assets](https://wiki.polkadot.com/learn/learn-dot/#__tabbed_2_1) page.

For more details about Paseo's role as the official Polkadot TestNet, see the [forum announcement](https://forum.polkadot.network/t/testnets-paseo-officially-becomes-the-polkadot-testnet-temporary-passet-hub-chain-for-smart-contracts-testing/13209).

## Other Networks

While Paseo serves as the default TestNet for most development workflows, the Polkadot ecosystem includes additional networks for specialized use cases.

### Kusama Network

Kusama is Polkadot's "canary" network—an experimental, production-grade environment with real economic value. Unlike TestNets, Kusama operates as a live network with actual incentives and economic consequences. It moves faster than Polkadot and has lower barriers to entry, making it suitable for teams that want to deploy in a real economic environment before moving to Polkadot.

Kusama is ideal for:

- Teams that want to test with real economic incentives
- Projects seeking a faster-moving governance and upgrade cycle
- Experiments and innovations that may eventually move to Polkadot

The native token for Kusama is KSM. For more information about KSM, visit the [Native Assets](https://wiki.polkadot.com/kusama/kusama-getting-started/) page.

### Westend

Westend is a protocol-focused TestNet maintained by Parity Technologies, primarily used for testing low-level Polkadot protocol changes, runtime upgrades, and infrastructure updates before they reach Kusama or Polkadot. Unlike Paseo, Westend is intentionally unstable and receives cutting-edge protocol changes first, making it better suited to core protocol development and infrastructure testing than to parachain or dApp development.

**Important**: Most external developers should **not** use Westend; instead, they should use Paseo TestNet. Westend's onboarding process is not clearly documented, and it is designed for infrastructure operators rather than general parachain teams. **Even if you plan to deploy to Kusama, you should test on Paseo TestNet, not Westend.**

Westend is primarily relevant for:

- Core Polkadot protocol developers testing runtime changes
- Infrastructure operators and validators testing low-level integrations
- Teams that specifically need to test against upcoming protocol changes before they reach production networks

Unlike temporary test networks, Westend is permanent and is not reset to the genesis block, making it an ongoing environment for long-term protocol testing.

The native token for Westend is WND. TestNet tokens are available from the [Polkadot faucet](https://faucet.polkadot.io/). More details about WND can be found on the [Native Assets](https://wiki.polkadot.com/learn/learn-dot/#__tabbed_2_2) page.

## Local Test Networks

Local test networks are an essential part of the development cycle for blockchain developers using the Polkadot SDK. They allow for fast, iterative testing in controlled, private environments without connecting to public TestNets. Developers can quickly spin up local instances to experiment, debug, and validate their code before deploying to Paseo TestNet and ultimately to production. Two key tools for local network testing are Zombienet and Chopsticks.

### Zombienet

[Zombienet](https://github.com/paritytech/zombienet) is a flexible testing framework for Polkadot SDK-based blockchains. It enables developers to create and manage ephemeral, short-lived networks. This feature makes Zombienet particularly useful for quick iterations, as it allows you to run multiple local networks concurrently, mimicking different runtime conditions. Whether you're developing a parachain or testing your custom blockchain logic, Zombienet gives you the tools to automate local testing.

Key features of Zombienet include:

- Creating dynamic, local networks with different configurations.
- Running parachains and relay chains in a simulated environment.
- Efficient testing of network components like cross-chain messaging and governance.

Zombienet is ideal for developers looking to test quickly and thoroughly before moving to more resource-intensive public TestNets.

### Chopsticks

[Chopsticks](https://github.com/AcalaNetwork/chopsticks) is a tool designed to create forks of Polkadot SDK-based blockchains, allowing developers to interact with network forks as part of their testing process. This capability makes Chopsticks a powerful option for testing upgrades, runtime changes, or cross-chain applications in a forked network environment.

Key features of Chopsticks include:

- Forking live Polkadot SDK-based blockchains for isolated testing.
- Simulating cross-chain messages in a private, controlled setup.
- Debugging network behavior by interacting with the fork in real-time.

Chopsticks provides a controlled environment for developers to safely explore the effects of runtime changes. It ensures that network behavior is tested and verified before upgrades are deployed to live networks.


---

Page Title: Node and Runtime

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/node-and-runtime.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/node-and-runtime/
- Summary: Learn how Polkadot SDK-based nodes function, how the client and runtime are separated, and how they communicate using SCALE-encoded data.
- Word Count: 627; Token Estimate: 913
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:9c162ccb450ed6f80fc386b0bc00aba64e05fa4bc4560dcf0217438e0d4e1a9a

# Node and Runtime

## Introduction

Every blockchain platform relies on a decentralized network of computers, called nodes, that communicate with each other about transactions and blocks. In this context, a node refers to the software running on the connected devices rather than the physical or virtual machines in the network.

Polkadot SDK-based nodes consist of two main components, each with distinct responsibilities: the client (also called node) and the runtime.

If the system were a monolithic protocol, any modification would require updating the entire system. Instead, Polkadot achieves true upgradeability by defining an immutable meta-protocol (the client) and a protocol (the runtime) that can be upgraded independently.

This separation gives the [Polkadot relay chain](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/relay-chain/) and all connected [parachains](/polkadot-mkdocs/reference/parachains/) an evolutionary advantage over other blockchain platforms.

## Architectural Principles

The Polkadot SDK-based blockchain architecture is fundamentally built on two distinct yet interconnected components:

- Client (Meta-protocol):
    - Handles the foundational infrastructure of the blockchain.
    - Manages runtime execution, networking, consensus, and other off-chain components.
    - Provides an immutable base layer that ensures network stability.
    - Upgradable only through hard forks.

- Runtime (Protocol):
    - Defines the blockchain's state transition logic.
    - Determines the specific rules and behaviors of the blockchain.
    - Compiled to WebAssembly (Wasm) for platform-independent execution.
    - Capable of being upgraded without network-wide forking.

### Advantages of this Architecture

- **Forkless upgrades**: Runtime can be updated without disrupting the entire network.
- **Modularity**: Clear separation allows independent development of client and runtime.
- **Flexibility**: Enables rapid iteration and evolution of blockchain logic.
- **Performance**: WebAssembly compilation provides efficient, cross-platform execution.

## Node (Client)

The node, also known as the client, is the core component responsible for executing the Wasm runtime and orchestrating various essential blockchain components. It ensures the correct execution of the state transition function and manages multiple critical subsystems, including:

- **Wasm execution**: Runs the blockchain runtime, which defines the state transition rules.
- **Database management**: Stores blockchain data.
- **Networking**: Facilitates peer-to-peer communication, block propagation, and transaction gossiping.
- **Transaction pool (Mempool)**: Manages pending transactions before they are included in a block.
- **Consensus mechanism**: Ensures agreement on the blockchain state across nodes.
- **RPC services**: Provides external interfaces for applications and users to interact with the node.

## Runtime

The runtime is more than just a set of rules. It's the fundamental logic engine that defines a blockchain's entire behavior. In Polkadot SDK-based blockchains, the runtime represents a complete, self-contained description of the blockchain's state transition function.

### Characteristics

The runtime is distinguished by three key characteristics:

- **Business logic**: Defines the complete application-specific blockchain behavior.
- **WebAssembly compilation**: Ensures platform-independent, secure execution.
- **On-chain storage**: Stored within the blockchain's state, allowing dynamic updates.

### Key Functions

The runtime performs several critical functions, such as:

- Define state transition rules.
- Implement blockchain-specific logic.
- Manage account interactions.
- Control transaction processing.
- Define governance mechanisms.
- Handle custom pallets and modules.

## Communication Between Node and Runtime

The client and runtime communicate exclusively using [SCALE-encoded](/polkadot-mkdocs/reference/parachains/data-encoding/) communication. This ensures efficient and compact data exchange between the two components.

### Runtime APIs

The Runtime API consists of well-defined functions and constants a client assumes are implemented in the Runtime Wasm blob. These APIs enable the client to interact with the runtime to execute blockchain operations and retrieve information. The client invokes these APIs to:

- Build, execute, and finalize blocks.
- Access metadata.
- Access consensus related information.
- Handle transaction execution.

### Host Functions

During execution, the runtime can access certain external client functionalities via host functions. The specific functions the client exposes allow the runtime to perform operations outside the WebAssembly domain. Host functions enable the runtime to:

- Perform cryptographic operations.
- Access the current blockchain state.
- Handle storage modifications.
- Allocate memory.


---

Page Title: On-Chain Governance Overview

- Resolved Markdown: https://docs.polkadot.com/reference/governance.md
- Canonical (HTML): https://docs.polkadot.com/reference/governance/
- Summary: Discover Polkadot’s cutting-edge OpenGov system, enabling transparent, decentralized decision-making through direct democracy and flexible governance tracks.
- Word Count: 985; Token Estimate: 1514
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:9769884c594d3d094436b1b74488ef09ad07a3d96a6d59230613a682f590d9cd

# On-Chain Governance 

## Introduction

Polkadot’s governance system exemplifies decentralized decision-making, empowering its community of stakeholders to shape the network’s future through active participation. The latest evolution, OpenGov, builds on Polkadot’s foundation by providing a more inclusive and efficient governance model.

This guide will explain the principles and structure of OpenGov and walk you through its key components, such as Origins, Tracks, and Delegation. You will learn about improvements over earlier governance systems, including streamlined voting processes and enhanced stakeholder participation.

With OpenGov, Polkadot achieves a flexible, scalable, and democratic governance framework that allows multiple proposals to proceed simultaneously, ensuring the network evolves in alignment with its community's needs.

## Governance Evolution

Polkadot’s governance journey began with [Governance V1](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#governance-summary), a system that proved effective in managing treasury funds and protocol upgrades. However, it faced limitations, such as:

- Slow voting cycles, causing delays in decision-making.
- Inflexibility in handling multiple referendums, restricting scalability.

To address these challenges, Polkadot introduced OpenGov, a governance model designed for greater inclusivity, efficiency, and scalability. OpenGov replaces the centralized structures of Governance V1, such as the Council and Technical Committee, with a fully decentralized and dynamic framework.

For a full comparison of the historic and current governance models, visit the [Gov1 vs. Polkadot OpenGov](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#gov1-vs-polkadot-opengov) section of the Polkadot Wiki.

## OpenGov Key Features

OpenGov transforms Polkadot’s governance into a decentralized, stakeholder-driven model, eliminating centralized decision-making bodies like the Council. Key enhancements include:

- **Decentralization**: Shifts all decision-making power to the public, ensuring a more democratic process.
- **Enhanced delegation**: Allows users to delegate their votes to trusted experts across specific governance tracks.
- **Simultaneous referendums**: Multiple proposals can progress at once, enabling faster decision-making.
- **Polkadot Technical Fellowship**: A broad, community-driven group replacing the centralized Technical Committee.

This new system ensures Polkadot governance remains agile and inclusive, even as the ecosystem grows.

## Origins and Tracks

In OpenGov, origins and tracks are central to managing proposals and votes.

- **Origin**: Determines the authority level of a proposal (e.g., Treasury, Root) which decides the track of all referendums from that origin.
- **Track**: Define the procedural flow of a proposal, such as voting duration, approval thresholds, and enactment timelines.

Developers must be aware that referendums from different origins and tracks will take varying amounts of time to reach approval and enactment. The [Polkadot Technical Fellowship](https://wiki.polkadot.com/learn/learn-polkadot-technical-fellowship/) has the option to shorten this timeline by whitelisting a proposal and allowing it to be enacted through the [Whitelist Caller](https://wiki.polkadot.com/learn/learn-polkadot-opengov-origins/#whitelisted-caller) origin.

Visit [Origins and Tracks Info](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#origins-and-tracks) for details on current origins and tracks, associated terminology, and parameters.

## Referendums

In OpenGov, anyone can submit a referendum, fostering an open and participatory system. The timeline for a referendum depends on the privilege level of the origin with more significant changes offering more time for community voting and participation before enactment. 

The timeline for an individual referendum includes four distinct periods:

- **Lead-in**: A minimum amount of time to allow for community participation, available room in the origin, and payment of the decision deposit. Voting is open during this period.
- **Decision**: Voting continues.
- **Confirmation**: Referendum must meet [approval and support](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#approval-and-support) criteria during entire period to avoid rejection.
- **Enactment**: Changes approved by the referendum are executed.

### Vote on Referendums

Voters can vote with their tokens on each referendum. Polkadot uses a voluntary token locking mechanism, called conviction voting, as a way for voters to increase their voting power. A token holder signals they have a stronger preference for approving a proposal based upon their willingness to lock up tokens. Longer voluntary token locks are seen as a signal of continual approval and translate to increased voting weight.

See [Voting on a Referendum](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#voting-on-a-referendum) for a deeper look at conviction voting and related token locks.

### Delegate Voting Power

The OpenGov system also supports multi-role delegations, allowing token holders to assign their voting power on different tracks to entities with expertise in those areas. 

For example, if a token holder lacks the technical knowledge to evaluate proposals on the [Root track](https://wiki.polkadot.com/learn/learn-polkadot-opengov-origins/#root), they can delegate their voting power for that track to an expert they trust to vote in the best interest of the network. This ensures informed decision-making across tracks while maintaining flexibility for token holders.

Visit [Multirole Delegation](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#multirole-delegation) for more details on delegating voting power.

### Cancel a Referendum

Polkadot OpenGov has two origins for rejecting ongoing referendums: 

- [**Referendum Canceller**](https://wiki.polkadot.com/learn/learn-polkadot-opengov-origins/#referendum-canceller): Cancels an active referendum when non-malicious errors occur and refunds the deposits to the originators.
- [**Referendum Killer**](https://wiki.polkadot.com/learn/learn-polkadot-opengov-origins/#referendum-killer): Used for urgent, malicious cases this origin instantly terminates an active referendum and slashes deposits.

See [Cancelling, Killing, and Blacklisting](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#cancelling-killing--blacklisting) for additional information on rejecting referendums.

## Additional Resources

- **[Democracy pallet](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/substrate/frame/democracy/src)**: Handles administration of general stakeholder voting.
- **[Gov2: Polkadot’s Next Generation of Decentralised Governance](https://medium.com/polkadot-network/gov2-polkadots-next-generation-of-decentralised-governance-4d9ef657d11b)**: Medium article by Gavin Wood.
- **[Polkadot Direction](https://matrix.to/#/#Polkadot-Direction:parity.io)**: Matrix Element client.
- **[Polkassembly](https://polkadot.polkassembly.io/)**: OpenGov dashboard and UI.
- **[Polkadot.js Apps Governance](https://polkadot.js.org/apps/#/referenda)**: Overview of active referendums.


---

Page Title: Opening HRMP Channels Between Parachains

- Resolved Markdown: https://docs.polkadot.com/parachains/interoperability/channels-between-parachains.md
- Canonical (HTML): https://docs.polkadot.com/parachains/interoperability/channels-between-parachains/
- Summary: Learn how to open HRMP channels between parachains on Polkadot. Discover the step-by-step process for establishing uni- and bidirectional communication.
- Word Count: 1563; Token Estimate: 2314
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:15c86dcd3ecc5f9bf97c26e0377bb437a61d591d930463a5218a4806b8d367d9

# Opening HRMP Channels Between Parachains

## Introduction

For establishing communication channels between parachains on the Polkadot network using the Horizontal Relay-routed Message Passing (HRMP) protocol, the following steps are required:

1. **Channel request**: The parachain that wants to open an HRMP channel must make a request to the parachain it wishes to have an open channel with.
2. **Channel acceptance**: The other parachain must then accept this request to complete the channel establishment.

This process results in a unidirectional HRMP channel, where messages can flow in only one direction between the two parachains.

An additional HRMP channel must be established in the opposite direction to enable bidirectional communication. This requires repeating the request and acceptance process but with the parachains reversing their roles.

Once both unidirectional channels are established, the parachains can send messages back and forth freely through the bidirectional HRMP communication channel.

## Prerequisites

Before proceeding, ensure you meet the following requirements:

- Blockchain network with a relay chain and at least two connected parachains.
- Wallet with sufficient funds to execute transactions on the participant chains.

## Procedure to Initiate an HRMP Channel

This example will demonstrate how to open a channel between parachain 2500 and parachain 2600, using Rococo Local as the relay chain.

### Fund Sender Sovereign Account


The [sovereign account](https://github.com/polkadot-fellows/xcm-format/blob/10726875bd3016c5e528c85ed6e82415e4b847d7/README.md?plain=1#L50) for parachain 2500 on the relay chain must be funded so it can take care of any XCM transact fees.

Use [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) UI to connect to the relay chain and transfer funds from your account to the parachain 2500 sovereign account.

![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-01.webp)

??? note "Calculating Parachain Sovereign Account"
    To generate the sovereign account address for a parachain, you'll need to follow these steps:

    1. Determine if the parachain is an "up/down" chain (parent or child) or a "sibling" chain:

        - Up/down chains use the prefix `0x70617261` (which decodes to `b"para"`).
        - Sibling chains use the prefix `0x7369626c` (which decodes to `b"sibl"`).

    2. Calculate the u32 scale encoded value of the parachain ID:

        - Parachain 2500 would be encoded as `c4090000`.

    3. Combine the prefix and parachain ID encoding to form the full sovereign account address:

        The sovereign account of parachain 2500 in relay chain will be `0x70617261c4090000000000000000000000000000000000000000000000000000`
        and the SS58 format of this address is `5Ec4AhPSY2GEE4VoHUVheqv5wwq2C1HMKa7c9fVJ1WKivX1Y`.

    To perform this conversion, you can also use the **"Para ID" to Address** section in [Substrate Utilities](https://www.shawntabrizi.com/substrate-js-utilities/).

### Create Channel Opening Extrinsic

1. In Polkadot.js Apps, connect to the relay chain, navigate to the **Developer** dropdown and select the **Extrinsics** option.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-02.webp)

2. Construct an `hrmpInitOpenChannel` extrinsic call:

    1. Select the **`hrmp`** pallet.
    2. Choose the **`hrmpInitOpenChannel`** extrinsic.
    3. Fill in the parameters:
        - **`recipient`**: Parachain ID of the target chain (in this case, 2600).
        - **`proposedMaxCapacity`**: Max number of messages that can be pending in the channel at once.
        - **`proposedMaxMessageSize`**: Max message size that could be put into the channel.
    4. Copy the encoded call data.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-01.webp)

    The encoded call data for opening a channel with parachain 2600 is `0x3c00280a00000800000000001000`.

### Craft and Submit the XCM Message from the Sender

To initiate the HRMP channel opening process, you need to create an XCM message that includes the encoded `hrmpInitOpenChannel` call data from the previous step. This message will be sent from your parachain to the relay chain.

This example uses the `sudo` pallet to dispatch the extrinsic. Verify the XCM configuration of the parachain you're working with and ensure you're using an origin with the necessary privileges to execute the `polkadotXcm.send` extrinsic.

The XCM message should contain the following instructions:

- **`WithdrawAsset`**: Withdraws assets from the origin's ownership and places them in the Holding Register.
- **`BuyExecution`**: Pays for the execution of the current message using the assets in the Holding Register.
- **`Transact`**: Execute the encoded transaction call.
- **`RefundSurplus`**: Increases the Refunded Weight Register to the value of the Surplus Weight Register, attempting to reclaim any excess fees paid via BuyExecution.
- **`DepositAsset`**: Subtracts assets from the Holding Register and deposits equivalent on-chain assets under the specified beneficiary's ownership.

!!!note
    For more detailed information about XCM's functionality, complexities, and instruction set, refer to the [xcm-format](https://github.com/polkadot-fellows/xcm-format) documentation.

In essence, this process withdraws funds from the parachain's sovereign account to the XCVM Holding Register, then uses these funds to purchase execution time for the XCM `Transact` instruction, executes `Transact`, refunds any unused execution time and deposits any remaining funds into a specified account.

To send the XCM message to the relay chain, connect to parachain 2500 in Polkadot.js Apps. Fill in the required parameters as shown in the image below, ensuring that you:

1. Replace the **`call`** field with your encoded `hrmpInitOpenChannel` call data from the previous step.
2. Use the correct beneficiary information.
3. Click the **Submit Transaction** button to dispatch the XCM message to the relay chain.

![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-02.webp)

!!! note
    The exact process and parameters for submitting this XCM message may vary depending on your specific parachain and relay chain configurations. Always refer to the most current documentation for your particular network setup.

After submitting the XCM message to initiate the HRMP channel opening, you should verify that the request was successful. Follow these steps to check the status of your channel request:

1. Using Polkadot.js Apps, connect to the relay chain and navigate to the **Developer** dropdown, then select the **Chain state** option.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-03.webp)

2. Query the HRMP open channel requests:

    1. Select **`hrmp`**.
    2. Choose the **`hrmpOpenChannelRequests`** call.
    3. Click the **+** button to execute the query.
    4. Check the status of all pending channel requests.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-03.webp)

If your channel request was successful, you should see an entry for your parachain ID in the list of open channel requests. This confirms that your request has been properly registered on the relay chain and is awaiting acceptance by the target parachain.

## Procedure to Accept an HRMP Channel

For the channel to be fully established, the target parachain must accept the channel request by submitting an XCM message to the relay chain.

### Fund Receiver Sovereign Account

Before proceeding, ensure that the sovereign account of parachain 2600 on the relay chain is funded. This account will be responsible for covering any XCM transact fees.
To fund the account, follow the same process described in the previous section, [Fund Sovereign Account](#fund-sender-sovereign-account).

### Create Channel Accepting Extrinsic

1. In Polkadot.js Apps, connect to the relay chain, navigate to the **Developer** dropdown and select the **Extrinsics** option.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-02.webp)

2. Construct an `hrmpAcceptOpenChannel` extrinsic call:

    1. Select the **`hrmp`** pallet.
    2. Choose the **`hrmpAcceptOpenChannel`** extrinsic.
    3. Fill in the parameters:
        - **`sender`**: Parachain ID of the requesting chain (in this case, 2500).
    4. Copy the encoded call data.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-04.webp)

    The encoded call data for accepting a channel with parachain 2500 should be `0x3c01c4090000`.

### Craft and Submit the XCM Message from the Receiver

To accept the HRMP channel opening, you need to create and submit an XCM message that includes the encoded `hrmpAcceptOpenChannel` call data from the previous step. This process is similar to the one described in the previous section, [Craft and Submit the XCM Message](#craft-and-submit-the-xcm-message-from-the-sender), with a few key differences:

- Use the encoded call data for `hrmpAcceptOpenChannel` obtained in step 2 of this section.
- In the last XCM instruction (DepositAsset), set the beneficiary to parachain 2600's sovereign account to receive any surplus funds.

To send the XCM message to the relay chain, connect to parachain 2600 in Polkadot.js Apps. Fill in the required parameters as shown in the image below, ensuring that you:

1. Replace the **`call`** field with your encoded `hrmpAcceptOpenChannel` call data from the previous step.
2. Use the correct beneficiary information.
3. Click the **Submit Transaction** button to dispatch the XCM message to the relay chain.

![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-05.webp)

After submitting the XCM message to accept the HRMP channel opening, verify that the channel has been set up correctly.

1. Using Polkadot.js Apps, connect to the relay chain and navigate to the **Developer** dropdown, then select the **Chain state** option.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-01.webp)

2. Query the HRMP channels:

    1. Select **`hrmp`**.
    2. Choose the **`hrmpChannels`** call.
    3. Click the **+** button to execute the query.
    4. Check the status of the opened channel.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-between-parachains/parachains-06.webp)

If the channel has been successfully established, you should see the channel details in the query results.

By following these steps, you will have successfully accepted the HRMP channel request and established a unidirectional channel between the two parachains.


!!! note
    Remember that for full bidirectional communication, you'll need to repeat this process in the opposite direction, with parachain 2600 initiating a channel request to parachain 2500.


---

Page Title: Opening HRMP Channels with System Parachains

- Resolved Markdown: https://docs.polkadot.com/parachains/interoperability/channels-with-system-parachains.md
- Canonical (HTML): https://docs.polkadot.com/parachains/interoperability/channels-with-system-parachains/
- Summary: Learn how to open HRMP channels with Polkadot system parachains. Discover the process for establishing bi-directional communication using a single XCM message.
- Word Count: 891; Token Estimate: 1429
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:6d58cb172aee1aa56d26e29e397b8f27d8ab85510fe441e21c5798946e67a9fb

# Opening HRMP Channels with System Parachains

## Introduction

While establishing Horizontal Relay-routed Message Passing (HRMP) channels between regular parachains involves a two-step request and acceptance procedure, opening channels with system parachains follows a more straightforward approach.

System parachains are specialized chains that provide core functionality to the Polkadot network. Examples include Asset Hub for cross-chain asset transfers and Bridge Hub for connecting to external networks. Given their critical role, establishing communication channels with these system parachains has been optimized for efficiency and ease of use.

Any parachain can establish a bidirectional channel with a system chain through a single operation, requiring just one XCM message from the parachain to the relay chain.

## Prerequisites

To successfully complete this process, you'll need to have the following in place:

- Access to a blockchain network consisting of:
    - A relay chain
    - A parachain
    - An Asset Hub system chain
- A wallet containing enough funds to cover transaction fees on each of the participating chains.

## Procedure to Establish an HRMP Channel

This guide demonstrates opening an HRMP channel between parachain 2500 and system chain Asset Hub (parachain 1000) on the Rococo Local relay chain.

### Fund Parachain Sovereign Account

The [sovereign account](https://github.com/polkadot-fellows/xcm-format/blob/10726875bd3016c5e528c85ed6e82415e4b847d7/README.md?plain=1#L50) for parachain 2500 on the relay chain must be funded so it can take care of any XCM transact fees.

Use [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) UI to connect to the relay chain and transfer funds from your account to the parachain 2500 sovereign account.

![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-01.webp)

??? note "Calculating Parachain Sovereign Account"
    To generate the sovereign account address for a parachain, you'll need to follow these steps:

    1. Determine if the parachain is an "up/down" chain (parent or child) or a "sibling" chain:

        - Up/down chains use the prefix `0x70617261` (which decodes to `b"para"`).
        - Sibling chains use the prefix `0x7369626c` (which decodes to `b"sibl"`).

    2. Calculate the u32 scale encoded value of the parachain ID:

        - Parachain 2500 would be encoded as `c4090000`.

    3. Combine the prefix and parachain ID encoding to form the full sovereign account address:

        The sovereign account of parachain 2500 in relay chain will be `0x70617261c4090000000000000000000000000000000000000000000000000000`
        and the SS58 format of this address is `5Ec4AhPSY2GEE4VoHUVheqv5wwq2C1HMKa7c9fVJ1WKivX1Y`.
    
    To perform this conversion, you can also use the **"Para ID" to Address** section in [Substrate Utilities](https://www.shawntabrizi.com/substrate-js-utilities/).

### Create Establish Channel with System Extrinsic

1. In Polkadot.js Apps, connect to the relay chain, navigate to the **Developer** dropdown and select the **Extrinsics** option.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-02.webp)

2. Construct an `establish_channel_with_system` extrinsic call:

    1. Select the **`hrmp`** pallet.
    2. Choose the **`establish_channel_with_system`** extrinsic.
    3. Fill in the parameters:
        - **`target_system_chain`**: Parachain ID of the target system chain (in this case, 1000).
    4. Copy the encoded call data.
    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-with-system-parachains/system-parachains-01.webp)

    The encoded call data for establishing a channel with system parachain 1000 should be `0x3c0ae8030000`.

### Craft and Submit the XCM Message

Connect to parachain 2500 using Polkadot.js Apps to send the XCM message to the relay chain. Input the necessary parameters as illustrated in the image below. Make sure to:

1. Insert your previously encoded `establish_channel_with_system` call data into the **`call`** field.
2. Provide beneficiary details.
3. Dispatch the XCM message to the relay chain by clicking the **Submit Transaction** button.

![](/polkadot-mkdocs/images/parachains/interoperability/channels-with-system-parachains/system-parachains-02.webp)

!!! note
    The exact process and parameters for submitting this XCM message may vary depending on your specific parachain and relay chain configurations. Always refer to the most current documentation for your particular network setup.

After successfully submitting the XCM message to the relay chain, two [`HrmpSystemChannelOpened`](https://paritytech.github.io/polkadot-sdk/master/polkadot_runtime_parachains/hrmp/pallet/enum.Event.html#variant.HrmpSystemChannelOpened) events are emitted, indicating that the channels are now present in storage under [`HrmpOpenChannelRequests`](https://paritytech.github.io/polkadot-sdk/master/polkadot_runtime_parachains/hrmp/pallet/storage_types/struct.HrmpOpenChannelRequests.html). However, the channels are not actually set up until the start of the next session, at which point bidirectional communication between parachain 2500 and system chain 1000 is established.

To verify this, wait for the next session and then follow these steps:

1. Using Polkadot.js Apps, connect to the relay chain and navigate to the **Developer** dropdown, then select **Chain state**.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-common/channels-03.webp)

2. Query the HRMP channels:

    1. Select **`hrmp`** from the options.
    2. Choose the **`hrmpChannels`** call.
    3. Click the **+** button to execute the query.

    ![](/polkadot-mkdocs/images/parachains/interoperability/channels-with-system-parachains/system-parachains-03.webp)
    
3. Examine the query results. You should see output similar to the following:

    ```json
    [
        [
            [
                {
                    "sender": 1000,
                    "recipient": 2500
                }
            ],
            {
                "maxCapacity": 8,
                "maxTotalSize": 8192,
                "maxMessageSize": 1048576,
                "msgCount": 0,
                "totalSize": 0,
                "mqcHead": null,
                "senderDeposit": 0,
                "recipientDeposit": 0
            }
        ],
        [
            [
                {
                    "sender": 2500,
                    "recipient": 1000
                }
            ],
            {
                "maxCapacity": 8,
                "maxTotalSize": 8192,
                "maxMessageSize": 1048576,
                "msgCount": 0,
                "totalSize": 0,
                "mqcHead": null,
                "senderDeposit": 0,
                "recipientDeposit": 0
            }
        ]
    ]
    ```

The output confirms the successful establishment of two HRMP channels:

- From chain 1000 (system chain) to chain 2500 (parachain).
- From chain 2500 (parachain) to chain 1000 (system chain).

This bidirectional channel enables direct communication between the system chain and the parachain, allowing for cross-chain message passing.


---

Page Title: Overview of FRAME

- Resolved Markdown: https://docs.polkadot.com/parachains/customize-runtime.md
- Canonical (HTML): https://docs.polkadot.com/parachains/customize-runtime/
- Summary: Learn how Polkadot SDK’s FRAME framework simplifies blockchain development with modular pallets and support libraries for efficient runtime design.
- Word Count: 1097; Token Estimate: 1804
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:6ed26f2f0189a1b170dd5451fa022fb3756e8edab67675ee7229a84d8f63c0b7

# Customize Your Runtime

## Introduction

A blockchain runtime is more than just a fixed set of rules—it's a dynamic foundation that you can shape to match your specific needs. With Polkadot SDK's [FRAME (Framework for Runtime Aggregation of Modularized Entities)](/polkadot-mkdocs/reference/glossary/#frame-framework-for-runtime-aggregation-of-modularized-entities), customizing your runtime is straightforward and modular. Instead of building everything from scratch, you combine pre-built pallets with your own custom logic to create a runtime suited to your blockchain's purpose.

This overview explains how runtime customization works, introduces the building blocks you'll use, and guides you through the key patterns for extending your runtime.

## Understanding Your Runtime

The runtime is the core logic of your blockchain—it processes transactions, manages state, and enforces the rules that govern your network. When a transaction arrives at your blockchain, the [`frame_executive`](https://paritytech.github.io/polkadot-sdk/master/frame_executive/index.html) pallet receives it and routes it to the appropriate pallet for execution.

Think of your runtime as a collection of specialized modules, each handling a different aspect of your blockchain. Need token balances? Use the Balances pallet. Want governance? Add the Governance pallets. Need something custom? Create your own pallet. By mixing and matching these modules, you build a runtime that's efficient, secure, and tailored to your use case.

## Runtime Architecture

The following diagram shows how FRAME components work together to form your runtime:

![](/polkadot-mkdocs/images/parachains/customize-runtime/index/frame-overview-01.webp)

The main components are:

- **`frame_executive`**: Routes all incoming transactions to the correct pallet for execution.
- **Pallets**: Domain-specific modules that implement your blockchain's features and business logic.
- **`frame_system`**: Provides core runtime primitives and storage.
- **`frame_support`**: Utilities and macros that simplify pallet development.

## Building Blocks: Pallets

[Pallets](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/frame_runtime/pallet/index.html) are the fundamental units of runtime customization. Each pallet encapsulates specific functionality and can be independently developed, tested, and integrated.

A pallet can implement virtually any blockchain feature you need:

- Expose new transactions that users can submit.
- Store data on-chain.
- Enforce business rules and validation logic.
- Emit events to notify users of state changes.
- Handle errors gracefully.

### Pre-Built Pallets vs. Custom Pallets

FRAME provides a comprehensive library of [pre-built pallets](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/substrate/frame) for common blockchain features, including consensus, staking, balances, governance, and more. These pallets are battle-tested, optimized, and ready to use.

However, you're not limited to pre-built functionality. When pre-built pallets don't meet your needs, you can create custom pallets with entirely custom logic. The real power of FRAME is the flexibility to use pre-built modules for standard features while building your own for unique requirements.

### Pallet Structure

FRAME uses Rust macros extensively, allowing you to focus on your pallet's logic while the framework handles boilerplate and integration code.

A typical pallet looks like this:

```rust
pub use pallet::*;

#[frame_support::pallet]
pub mod pallet {
  use frame_support::pallet_prelude::*;
  use frame_system::pallet_prelude::*;

  #[pallet::pallet]
  #[pallet::generate_store(pub(super) trait Store)]
  pub struct Pallet<T>(_);

  #[pallet::config]  // snip
  #[pallet::event]   // snip
  #[pallet::error]   // snip
  #[pallet::storage] // snip
  #[pallet::call]    // snip
}
```

Every pallet can implement these core macros:

- **`#[frame_support::pallet]`**: Marks your module as a FRAME pallet.
- **`#[pallet::pallet]`**: Designates the struct that holds pallet metadata.
- **`#[pallet::config]`**: Defines configuration and associated types.
- **`#[pallet::event]`**: Defines events emitted by your pallet.
- **`#[pallet::error]`**: Defines error types your pallet can return.
- **`#[pallet::storage]`**: Defines on-chain storage items.
- **`#[pallet::call]`**: Defines dispatchable functions (transactions).

For a comprehensive reference, see the [`pallet_macros` documentation](https://paritytech.github.io/polkadot-sdk/master/frame_support/pallet_macros/index.html).

## How Runtime Customization Works

Customizing your runtime typically follows these patterns:

**Adding Pre-Built Pallets**: Select pallets from the FRAME library and integrate them into your runtime configuration. This is the fastest way to add functionality.

**Creating Custom Pallets**: Write custom pallets for features that don't exist in the pre-built library. Custom pallets follow the same structure as pre-built ones and integrate seamlessly.

**Combining Multiple Pallets**: Layer multiple pallets together to create complex behaviors. Pallets can call each other and share storage when needed.

**Configuring Pallet Parameters**: Most pallets are configurable—you can adjust their behavior through configuration traits without modifying their code.

The following diagram illustrates how pallets combine to form a complete runtime:

![](/polkadot-mkdocs/images/parachains/customize-runtime/index/frame-overview-02.webp)

## Starting Templates

The easiest way to begin customizing your runtime is with a starter template. These templates provide a pre-configured foundation so you can focus on customization rather than setup.

- **[Polkadot SDK Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template)**: The recommended choice for most developers, it includes pre-configured pallets for common features (balances, block production, governance), a complete runtime setup, and built-in parachain consensus support. This template offers the best balance of features and learning opportunities.

- **[Polkadot SDK Minimal Template](https://github.com/paritytech/polkadot-sdk-minimal-template)**: Provides a bare-bones runtime with only essential components. Choose this if you want maximum flexibility and prefer building from a clean slate.

- **[Polkadot SDK Solochain Template](https://github.com/paritytech/polkadot-sdk/tree/master/templates/solochain)**: Designed for building standalone blockchains with moderate features, simple consensus, and several core pallets. Use this if you want a sovereign blockchain independent of a relay chain.

- **[OpenZeppelin Runtime Templates](https://github.com/OpenZeppelin/polkadot-runtime-templates)**: Provides security-focused configurations following industry best practices. The [generic-template](https://github.com/OpenZeppelin/polkadot-runtime-templates/tree/main/generic-template) includes curated pallet selections and production-ready defaults—ideal if security is your top priority.

## Key Customization Scenarios

This section covers the most common customization patterns you'll encounter:

- **[Add Existing Pallets to Your Runtime](/polkadot-mkdocs/parachains/customize-runtime/add-existing-pallets/)**: Integrate pre-built pallets from the FRAME library with minimal configuration.

- **[Add Multiple Instances of a Pallet](/polkadot-mkdocs/parachains/customize-runtime/add-pallet-instances/)**: Run multiple instances of the same pallet with different configurations—useful for multi-token systems or parallel features.

- **[Add Smart Contract Functionality](/polkadot-mkdocs/parachains/customize-runtime/add-smart-contract-functionality/)**: Enable smart contract execution on your parachain using Contracts pallets.

- **[Create Custom Pallets](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/create-a-pallet/)**: Build entirely custom pallets for features unique to your blockchain.

- **[Test Your Runtime](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/pallet-testing/)**: Unit test pallets and mock complete runtimes to ensure everything works correctly.


---

Page Title: Overview of the Polkadot Relay Chain

- Resolved Markdown: https://docs.polkadot.com/reference/polkadot-hub/consensus-and-security/relay-chain.md
- Canonical (HTML): https://docs.polkadot.com/reference/polkadot-hub/consensus-and-security/relay-chain/
- Summary: Explore Polkadot's core architecture, including its multi-chain vision, shared security, and the DOT token's governance and staking roles.
- Word Count: 1740; Token Estimate: 2466
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:8183fd31d42d2564ca641334436eca3a5c13d836ccb8b210878d7493cabe0a6f

# Overview

## Introduction

Polkadot is a next-generation blockchain protocol designed to support a multi-chain future by enabling secure communication and interoperability between different blockchains. Built as a Layer-0 protocol, Polkadot introduces innovations like application-specific Layer-1 chains ([parachains](/polkadot-mkdocs/reference/parachains/)), shared security through [Nominated Proof of Stake (NPoS)](/polkadot-mkdocs/reference/glossary/#nominated-proof-of-stake-npos), and cross-chain interactions via its native [Cross-Consensus Messaging Format (XCM)](/polkadot-mkdocs/parachains/interoperability/get-started/).

This guide covers key aspects of Polkadot’s architecture, including its high-level protocol structure, blockspace commoditization, and the role of its native token, DOT, in governance, staking, and resource allocation.

## Polkadot 1.0

Polkadot 1.0 represents the state of Polkadot as of 2023, coinciding with the release of [Polkadot runtime v1.0.0](https://github.com/paritytech/polkadot/releases/tag/v1.0.0). This section will focus on Polkadot 1.0, along with philosophical insights into network resilience and blockspace.

As a Layer-0 blockchain, Polkadot contributes to the multi-chain vision through several key innovations and initiatives, including:

- **Application-specific Layer-1 blockchains (parachains)**: Polkadot's sharded network allows for parallel transaction processing, with shards that can have unique state transition functions, enabling custom-built L1 chains optimized for specific applications.

- **Shared security and scalability**: L1 chains connected to Polkadot benefit from its [Nominated Proof of Stake (NPoS)](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/#nominated-proof-of-stake) system, providing security out-of-the-box without the need to bootstrap their own.

- **Secure interoperability**: Polkadot's native interoperability enables seamless data and value exchange between parachains. This interoperability can also be used outside of the ecosystem for bridging with external networks.

- **Resilient infrastructure**: Decentralized and scalable, Polkadot ensures ongoing support for development and community initiatives via its on-chain [treasury](https://wiki.polkadot.com/learn/learn-polkadot-opengov-treasury/) and governance.

- **Rapid L1 development**: The [Polkadot SDK](/polkadot-mkdocs/reference/parachains/) allows fast, flexible creation and deployment of Layer-1 chains.

- **Cultivating the next generation of Web3 developers**: Polkadot supports the growth of Web3 core developers through initiatives such as.

    - [Polkadot Blockchain Academy](https://polkadot.academy/)
    - [EdX courses](https://www.edx.org/school/web3x)
    - Rust and Substrate courses (coming soon)

### High-Level Architecture

Polkadot features a chain that serves as the central component of the system. This chain is depicted as a ring encircled by several parachains that are connected to it.

According to Polkadot's design, any blockchain that can compile to WebAssembly (Wasm) and adheres to the Parachains Protocol becomes a parachain on the Polkadot network.

Here’s a high-level overview of the Polkadot protocol architecture:

![](/polkadot-mkdocs/images/reference/polkadot-hub/consensus-and-security/relay-chain/relay-chain-01.webp)

Parachains propose blocks to Polkadot validators, who check for availability and validity before finalizing them. With the relay chain providing security, collators—full nodes of parachains—can focus on their tasks without needing strong incentives.

The [Cross-Consensus Messaging Format (XCM)](/polkadot-mkdocs/parachains/interoperability/get-started/) allows parachains to exchange messages freely, leveraging the chain's security for trust-free communication.

In order to interact with chains that want to use their own finalization process (e.g., Bitcoin), Polkadot has [bridges](/polkadot-mkdocs/reference/parachains/interoperability/#bridges-connecting-external-networks) that offer two-way compatibility, meaning that transactions can be made between different parachains.

### Polkadot's Additional Functionalities

Historically, obtaining core slots on Polkadot chain relied upon crowdloans and auctions. Chain cores were leased through auctions for three-month periods, up to a maximum of two years. Crowdloans enabled users to securely lend funds to teams for lease deposits in exchange for pre-sale tokens, which is the only way to access slots on Polkadot 1.0. Auctions are now deprecated in favor of [coretime](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/agile-coretime/).

Additionally, the chain handles [staking](https://wiki.polkadot.com/learn/learn-staking/), [accounts](/polkadot-mkdocs/reference/parachains/accounts/), balances, and [governance](/polkadot-mkdocs/reference/governance/).

#### Agile Coretime

The new and more efficient way of obtaining core on Polkadot is to go through the process of purchasing coretime.

[Agile coretime](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/agile-coretime/) improves the efficient use of Polkadot's network resources and offers economic flexibility for developers, extending Polkadot's capabilities far beyond the original vision outlined in the [whitepaper](https://assets.polkadot.network/Polkadot-whitepaper.pdf)..

It enables parachains to purchase monthly "bulk" allocations of coretime (the time allocated for utilizing a core, measured in Polkadot relay chain blocks), ensuring heavy-duty parachains that can author a block every six seconds with [Asynchronous Backing](https://wiki.polkadot.com/learn/learn-async-backing/#asynchronous-backing) can reliably renew their coretime each month. Although six-second block times are now the default, parachains have the option of producing blocks less frequently.

Renewal orders are prioritized over new orders, offering stability against price fluctuations and helping parachains budget more effectively for project costs.

### Polkadot's Resilience

Decentralization is a vital component of blockchain networks, but it comes with trade-offs:

- An overly decentralized network may face challenges in reaching consensus and require significant energy to operate.
- Also, a network that achieves consensus quickly risks centralization, making it easier to manipulate or attack.

A network should be decentralized enough to prevent manipulative or malicious influence. In this sense, decentralization is a tool for achieving resilience.

Polkadot 1.0 currently achieves resilience through several strategies:

- **Nominated Proof of Stake (NPoS)**: Ensures that the stake per validator is maximized and evenly distributed among validators.

- **Decentralized nodes**: Designed to encourage operators to join the network. This program aims to expand and diversify the validators in the ecosystem who aim to become independent of the program during their term. Feel free to explore more about the program on the official [Decentralized Nodes](https://nodes.web3.foundation/) page.

- **On-chain treasury and governance**: Known as [OpenGov](/polkadot-mkdocs/reference/governance/), this system allows every decision to be made through public referenda, enabling any token holder to cast a vote.

### Polkadot's Blockspace

Polkadot 1.0’s design allows for the commoditization of blockspace.

Blockspace is a blockchain's capacity to finalize and commit operations, encompassing its security, computing, and storage capabilities. Its characteristics can vary across different blockchains, affecting security, flexibility, and availability.

- **Security**: Measures the robustness of blockspace in Proof of Stake (PoS) networks linked to the stake locked on validator nodes, the variance in stake among validators, and the total number of validators. It also considers social centralization (how many validators are owned by single operators) and physical centralization (how many validators run on the same service provider).

- **Flexibility**: Reflects the functionalities and types of data that can be stored, with high-quality data essential to avoid bottlenecks in critical processes.

- **Availability**: Indicates how easily users can access blockspace. It should be easily accessible, allowing diverse business models to thrive, ideally regulated by a marketplace based on demand and supplemented by options for "second-hand" blockspace.

Polkadot is built on core blockspace principles, but there's room for improvement. Tasks like balance transfers, staking, and governance are managed on the relay chain.

Delegating these responsibilities to [system chains](/polkadot-mkdocs/reference/polkadot-hub/#core-components) could enhance flexibility and allow the relay chain to concentrate on providing shared security and interoperability.

For more information about blockspace, read [Robert Habermeier’s technical blog post](https://www.rob.tech/blog/polkadot-blockspace-over-blockchains/).

## DOT Token

DOT is the native token of the Polkadot network, much like BTC for Bitcoin and Ether for the Ethereum blockchain. DOT has 10 decimals, uses the Planck base unit, and has a balance type of `u128`. The same is true for Kusama's KSM token with the exception of having 12 decimals.

### Redenomination of DOT
    
Polkadot conducted a community poll, which ended on 27 July 2020 at block 888,888, to decide whether to redenominate the DOT token. The stakeholders chose to redenominate the token, changing the value of 1 DOT from 1e12 plancks to 1e10 plancks.

Importantly, this did not affect the network's total number of base units (plancks); it only affects how a single DOT is represented. The redenomination became effective 72 hours after transfers were enabled, occurring at block 1,248,328 on 21 August 2020 around 16:50 UTC.

### The Planck Unit

The smallest unit of account balance on Polkadot SDK-based blockchains (such as Polkadot and Kusama) is called _Planck_, named after the Planck length, the smallest measurable distance in the physical universe.

Similar to how BTC's smallest unit is the Satoshi and ETH's is the Wei, Polkadot's native token DOT equals 1e10 Planck, while Kusama's native token KSM equals 1e12 Planck.

### Uses for DOT

DOT serves three primary functions within the Polkadot network:

- **Governance**: It is used to participate in the governance of the network.
- **Staking**: DOT is staked to support the network's operation and security.
- **Buying coretime**: Used to purchase coretime in-bulk or on-demand and access the  chain to benefit from Polkadot's security and interoperability.

Additionally, DOT can serve as a transferable token. For example, DOT, held in the treasury, can be allocated to teams developing projects that benefit the Polkadot ecosystem.

## JAM and the Road Ahead

The Join-Accumulate Machine (JAM) represents a transformative redesign of Polkadot's core architecture, envisioned as the successor to the current relay chain. Unlike traditional blockchain architectures, JAM introduces a unique computational model that processes work through two primary functions:

- **Join**: Handles data integration.
- **Accumulate**: Folds computations into the chain's state.

JAM removes many of the opinions and constraints of the current relay chain while maintaining its core security properties. Expected improvements include:

- **Permissionless code execution**: JAM is designed to be more generic and flexible, allowing for permissionless code execution through services that can be deployed without governance approval.
- **More effective block time utilization**: JAM's efficient pipeline processing model places the prior state root in block headers instead of the posterior state root, enabling more effective utilization of block time for computations.

This architectural evolution promises to enhance Polkadot's scalability and flexibility while maintaining robust security guarantees. JAM is planned to be rolled out to Polkadot as a single, complete upgrade rather than a stream of smaller updates. This approach seeks to minimize the developer overhead required to address any breaking changes.


---

Page Title: Parachains Overview

- Resolved Markdown: https://docs.polkadot.com/reference/parachains.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/
- Summary: Learn about parachains, specialized blockchains on Polkadot that gain shared security and interoperability. Discover how they work and the tools to build them.
- Word Count: 1025; Token Estimate: 1738
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:10a9d24233b190b0271699c619ef3711171c5e39db3a640040bf2be0a872ec34

# Parachains Overview

## Introduction

A parachain is a specialized blockchain that connects to the Polkadot relay chain, benefiting from shared security, interoperability, and scalability. Parachains are built using the [Polkadot SDK](https://github.com/paritytech/polkadot-sdk), a powerful toolkit written in Rust that provides everything needed to create custom blockchain logic while integrating seamlessly with the Polkadot network.

Unlike standalone blockchains that must bootstrap their own validator sets and security, parachains leverage Polkadot's pooled security model. This allows parachain developers to focus on their application-specific functionality rather than consensus and security infrastructure. Parachains can communicate with each other through Cross-Consensus Messaging (XCM), enabling seamless interoperability across the Polkadot ecosystem.

Key capabilities that parachains provide include:

- **Shared security**: Inherit security from Polkadot's validator set without maintaining your own.
- **Interoperability**: Communicate trustlessly with other parachains via XCM.
- **Scalability**: Process transactions in parallel with other parachains.
- **Customization**: Build application-specific logic tailored to your use case.
- **Upgradeability**: Upgrade runtime logic without hard forks.

## Polkadot SDK: Parachain Architecture

Building a parachain involves understanding and utilizing several key components of the Polkadot SDK:

![](/polkadot-mkdocs/images/reference/parachains/index/overview-01.webp)

- **[Substrate](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/substrate/index.html)**: The foundation providing core blockchain primitives and libraries.
- **[FRAME](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/frame_runtime/index.html)**: A modular framework for building your parachain's runtime logic.
- **[Cumulus](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/cumulus/index.html)**: Essential libraries and pallets that enable parachain functionality.
- **[XCM (Cross Consensus Messaging)](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/xcm/index.html)**: The messaging format for communicating with other parachains and the relay chain.
- **[Polkadot](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/polkadot_sdk/polkadot/index.html)**: The relay chain that provides security and coordination.

### Substrate: The Foundation

Substrate provides the core infrastructure that every parachain is built upon. It handles the low-level blockchain functionality, allowing you to focus on your application's unique features. Substrate includes implementations for networking, database management, consensus participation, and the execution environment for your runtime.

Every Polkadot SDK node consists of two main components:

- **Client (Host)**: Handles infrastructure services.

    - Native binary that runs on validator and collator nodes.
    - Executes the Wasm-compiled runtime.
    - Manages networking, database, mempool, and block production.
    - Interfaces with the relay chain for validation.

- **Runtime (State Transition Function)**: Contains your business logic.

    - Defines how your Polkadot SDK node processes transactions.
    - Compiled to [Wasm](https://webassembly.org/) for deterministic execution.
    - Stored on-chain and upgradeable via governance.

```mermaid
%%{init: {'flowchart': {'padding': 5, 'nodeSpacing': 50, 'rankSpacing': 10}}}%%
graph TB
    classDef title font-size:20px,font-weight:bold,stroke-width:0px
    classDef clientStyle font-size:16px,font-weight:bold
    classDef clientSubNodeStyle margin-top:10px
    classDef runtimeCallExecutorStyle padding-top:10px

    subgraph sg1[Parachain<br /> Node]
        direction TB

        I[RuntimeCall Executor]
        B[Wasm Runtime - STF]

        subgraph sg2[Client]
            direction TB
            C[Network and Blockchain<br/>Infrastructure Services<br/>+ Relay Chain Interface]
        end

        I --> B
    end

    class sg1 title
    class sg2 clientStyle
    class C clientSubNodeStyle
    class I runtimeCallExecutorStyle

```

### FRAME: Building Blocks for Your Runtime

FRAME provides modular components called [pallets](/polkadot-mkdocs/reference/glossary#pallet) that you can compose to build your parachain's runtime. Each pallet provides specific functionality that you can customize and configure for your needs. This modular approach allows you to quickly assemble complex functionality without writing everything from scratch.

```mermaid
graph LR
    subgraph SP["<b style='font-size:18px;'>Parachain Runtime</b>"]
        direction LR
        Timestamp ~~~ Aura ~~~ ParachainSystem
        Balances ~~~ TransactionPayment ~~~ Sudo
        subgraph Timestamp["Timestamp"]
            SS1[Custom Config]
        end
        subgraph Aura["Aura"]
            SS2[Custom Config]
        end
        subgraph ParachainSystem["Parachain System"]
            SS3[Custom Config]
        end
        subgraph Balances["Balances"]
            SS4[Custom Config]
        end
        subgraph TransactionPayment["Transaction Payment"]
            SS5[Custom Config]
        end
        subgraph Sudo["Sudo"]
            SS6[Custom Config]
        end
        style Timestamp stroke:#FF69B4
        style Aura stroke:#FF69B4
        style ParachainSystem stroke:#FF69B4
        style Balances stroke:#FF69B4
        style TransactionPayment stroke:#FF69B4
        style Sudo stroke:#FF69B4
        style SS1 stroke-dasharray: 5
        style SS2 stroke-dasharray: 5
        style SS3 stroke-dasharray: 5
        style SS4 stroke-dasharray: 5
        style SS5 stroke-dasharray: 5
        style SS6 stroke-dasharray: 5

    end
    subgraph AP["<b style='font-size:18px;'>Available FRAME Pallets</b>"]
        direction LR
        A1[Aura]~~~A2[Parachain<br>System]~~~A3[Transaction<br>Payment]~~~A4[Sudo]
        B1[Identity]~~~B2[Balances]~~~B3[Assets]~~~B4[EVM]
        C1[Timestamp]~~~C2[Staking]~~~C3[Contracts]~~~C4[and more...]
    end
    AP --> SP
```

### Cumulus: Parachain-Specific Functionality

Cumulus is what transforms a Polkadot SDK-based runtime into a parachain-capable runtime. It provides the essential components for communicating with the relay chain, participating in Polkadot's consensus, and handling parachain-specific operations like block validation and collation.

Key Cumulus components include:

- **Parachain system pallet**: Core parachain functionality and relay chain communication.
- **Collator consensus**: Block production logic for parachain collators.
- **Relay chain interface**: APIs for interacting with the Polkadot relay chain.
- **Validation data**: Handling proof-of-validity data required by relay chain validators.

## Where to Go Next

Building a parachain requires understanding the relationship between your chain and the Polkadot relay chain. The Polkadot SDK provides all the tools needed to design custom runtime logic, enable cross-chain communication, and deploy your parachain to production.

The following sections provide detailed guidance on each aspect of parachain development, from initial design through deployment and ongoing maintenance.

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Launch a Simple Parachain__

    ---

    Walk through the complete parachain launch flow: from setup and deployment to obtaining coretime.

    [:octicons-arrow-right-24: Deploy](/polkadot-mkdocs/parachains/launch-a-parachain/set-up-the-parachain-template/)


-   <span class="badge guide">Guide</span> __Customize Your Runtime__

    ---

    Design your parachain's runtime logic and choose appropriate pallets for your use case.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/customize-runtime/)

-   <span class="badge guide">Guide</span> __Interoperability__

    ---

    Implement XCM for trustless cross-chain communication with other parachains.

    [:octicons-arrow-right-24: Learn More](/polkadot-mkdocs/parachains/interoperability/get-started/)

-   <span class="badge guide">Guide</span> __Runtime Upgrades__

    ---

    Upgrade your parachain's runtime without hard forks using forkless upgrade mechanisms.

    [:octicons-arrow-right-24: Maintain](/polkadot-mkdocs/parachains/runtime-maintenance/runtime-upgrades/)

</div>


---

Page Title: ParaSpell XCM SDK

- Resolved Markdown: https://docs.polkadot.com/reference/tools/paraspell.md
- Canonical (HTML): https://docs.polkadot.com/reference/tools/paraspell/
- Summary: A powerful open-source library that simplifies XCM integration, enabling developers to easily build interoperable dApps on Polkadot.
- Word Count: 548; Token Estimate: 846
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:cffde2640a9e6aa0c2582916060fc3654de9584f1adb54a8342fba7ae64f1c42

# ParaSpell XCM SDK

## Introduction

[ParaSpell](https://paraspell.github.io/docs/) is a comprehensive suite of open-source tools designed to simplify cross-chain interactions within the Polkadot ecosystem. At its core, ParaSpell is dedicated to enhancing the functionality of the [XCM (Cross-Consensus Messaging)](/polkadot-mkdocs/parachains/interoperability/get-started/) protocol by providing developers with a unified and streamlined experience for building interoperable decentralized applications (dApps).

The primary goal of ParaSpell is to abstract away the complexities of the XCM protocol. While XCM is a powerful feature of the Polkadot network, its implementation can vary significantly between different parachains. ParaSpell addresses this challenge by providing a standardized set of tools that enable developers to easily integrate cross-chain functionality into their applications, saving valuable time and effort. ParaSpell is a "common good" software, meaning it is free, open-source, and dedicated to the growth of the Polkadot ecosystem.

The ParaSpell suite includes:

- **[XCM SDK](https://paraspell.xyz/#xcm-sdk)**: Provides a unified layer to incorporate XCM into decentralized applications, simplifying complex cross-chain interactions.
- **[XCM API](https://paraspell.xyz/#xcm-api)**: Offers an efficient, package-free approach to integrating XCM functionality while offloading heavy computing tasks, minimizing costs and improving application performance.
- **[XCM Router](https://paraspell.xyz/#xcm-router)**: Enables cross-chain asset swaps in a single command, allowing developers to send one asset type (such as DOT on Polkadot) and receive a different asset on another chain (like ASTR on Astar).
- **[XCM Analyser](https://paraspell.xyz/#xcm-analyser)**: Decodes and translates complex XCM multilocation data into readable information, supporting easier troubleshooting and debugging.
- **[XCM Visualizator](https://paraspell.xyz/#xcm-visualizator)**: A tool designed to give developers a clear, interactive view of XCM activity across the Polkadot ecosystem, providing insights into cross-chain communication flow.
- **[XCM Playground](https://paraspell.xyz/#try-it)**: An interactive playground for testing different XCM scenarios.

### ParaSpell XCM SDK

The [ParaSpell XCM SDK](https://paraspell.github.io/docs/sdk/getting-started.html) is a core component of the ParaSpell toolset and a foundational library for developers looking to leverage XCM in their applications. It is the first and only XCM SDK in the ecosystem to support both PolkadotJS and Polkadot API, providing developers with flexibility and choice.

The SDK simplifies the process of creating and sending XCM messages by providing a user-friendly builder pattern. This allows developers to construct complex XCM calls with just a few lines of code, reducing the likelihood of errors and ensuring that messages are constructed correctly.

By using the ParaSpell XCM SDK, developers can significantly accelerate their development workflow and build powerful, interoperable dApps that take full advantage of the Polkadot network's cross-chain capabilities.

## Install ParaSpell

If you want to use ParaSpell in your project you can add it as a dependency with the following command:

=== "npm"

    ```bash
    npm install --save @paraspell/sdk@12.8.8
    ```

=== "pnpm"

    ```bash
    pnpm add @paraspell/sdk@12.8.8
    ```

=== "yarn"

    ```bash
    yarn add @paraspell/sdk@12.8.8
    ```


## Where to Go Next

Explore more about ParaSpell through these resources:

<div class="grid cards" markdown>

-   <span class="badge tutorial">Tutorial</span> __Transfer Assets Between Parachains__

    ---

    Learn how to transfer assets across chains with ParaSpell.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/transfer-assets-parachains/)

</div>


---

Page Title: Polkadot Hub Overview

- Resolved Markdown: https://docs.polkadot.com/reference/polkadot-hub.md
- Canonical (HTML): https://docs.polkadot.com/reference/polkadot-hub/
- Summary: Learn how Polkadot Hub serves as the entry point to Polkadot, providing access to smart contracts, staking, governance, identity management, and cross-ecosystem interoperability.
- Word Count: 832; Token Estimate: 1257
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:79473ced5d195314f56153825976837420cc3bb5c5be3fadb8c4acb768167841

## Introduction

Polkadot Hub is the entry point for all users and application developers to Polkadot. It provides access to essential Web3 services, including smart contracts, staking, governance, identity management, and cross-ecosystem interoperability—without requiring you to deploy or manage a parachain.

The Hub encompasses a set of core functionality that enables developers and users to build and interact with applications on Polkadot. This specialized system of parachains and services works together seamlessly to deliver a unified platform experience. The modular approach lets you interact with services optimized for specific use cases, while still benefiting from Polkadot's shared security.

## Polkadot Hub Capabilities

Whether you're just getting started or building complex applications, the Hub supports the ability to:
 
- Hold, send, and receive DOT and other assets across the network.
- Stake DOT to participate in network security and earn rewards.
- Vote in governance referendums and shape Polkadot's future.
- Create both fungible and non-fungible tokens and assets for your projects.
- Pay transaction fees in any asset, not just DOT.
- Register as an individual and establish your on-chain identity.

For more sophisticated development use cases, the Hub enables you to:

- Deploy Ethereum-compatible smart contracts using Solidity or other EVM languages.
- Build decentralized applications that leverage Polkadot's security and interoperability.
- Create and manage fungible tokens and NFTs with low fees and flexible operations.
- Manage cross-chain interactions through XCM messaging with other parachains.
- Set verified identities and apply for network opportunities like the Ambassador Program.
- Join collectives and participate in governance organizations with specialized roles.

## Core Components

The Polkadot Hub consists of several specialized system parachains and services working together as described in the following sections. 

### Smart Contracts

[Smart Contracts](/polkadot-mkdocs/reference/polkadot-hub/smart-contracts/) on Polkadot Hub enable developers to deploy Ethereum-compatible smart contracts written in Solidity and other familiar EVM languages. Build decentralized applications with full access to Polkadot's security, interoperability, and cross-chain capabilities. Smart contracts on the Hub benefit from lower fees and integration with native Polkadot features, such as identity management and asset operations.

### Asset Management

[Asset Management](/polkadot-mkdocs/reference/polkadot-hub/assets/) provides the foundation for on-chain asset management. Create, manage, and transfer fungible tokens and NFTs across the ecosystem. Asset Management offers significantly lower transaction fees—approximately one-tenth the cost of relay chain transactions—and reduced deposit requirements, making it ideal for projects managing digital assets at scale. It also enables payment of transaction fees in non-native assets, providing developers and users with greater flexibility.

### People Chain

[People Chain](/polkadot-mkdocs/reference/polkadot-hub/people-and-identity/) powers Polkadot's decentralized identity system. Establish verifiable on-chain identities, control disclosure of personal information, and receive verification from trusted registrars. People Chain enables secure identity management with hierarchical sub-account support, forming the foundation for trusted interactions throughout the ecosystem.

### Bridge Hub

[Bridge Hub](/polkadot-mkdocs/reference/polkadot-hub/bridging/) facilitates trustless interactions between Polkadot and external blockchains like Ethereum and Kusama. Through implementations such as Snowbridge, Bridge Hub enables secure cross-chain communication via on-chain light clients and trustless relayers. This component ensures seamless interoperability beyond the Polkadot ecosystem.

### Consensus & Security

[Consensus and Security](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/relay-chain/) covers the fundamental mechanisms that protect the network. Learn about validator participation, how the relay chain validates all transactions, and the cryptoeconomic incentives that secure Polkadot. Understanding these mechanisms is essential for validators and anyone building critical infrastructure on the network.

### Collectives & DAOs

[Collectives and DAOs](/polkadot-mkdocs/reference/polkadot-hub/collectives-and-daos/) enable specialized governance organizations within Polkadot. Participate in collective membership, manage treasury operations, and engage in coordinated decision-making with groups aligned around specific purposes. This functionality supports the creation of autonomous organizations on Polkadot.

## Where to Go Next

Consider the following resources to explore specific Hub functionality.

<div class="grid cards" markdown>

- <span class="badge learn">Learn</span> **Smart Contracts**

    ---

    Deploy Ethereum-compatible smart contracts and build decentralized applications.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/smart-contracts/)

- <span class="badge learn">Learn</span> **Asset Management**

    ---

    Manage fungible tokens and NFTs with low fees and flexible asset operations.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/assets/)

- <span class="badge learn">Learn</span> **People Chain**

    ---

    Establish and verify decentralized identities for trusted interactions on Polkadot.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/people-and-identity/)

- <span class="badge learn">Learn</span> **Bridge Hub**

    ---

    Facilitate trustless cross-chain interactions with Ethereum and other blockchains.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/bridging/)

- <span class="badge learn">Learn</span> **Consensus & Security**

    ---

    Understand how Polkadot validates transactions and secures the network.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/relay-chain/)

- <span class="badge learn">Learn</span> **Collectives & DAOs**

    ---

    Participate in specialized governance organizations with coordinated decision-making.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/collectives-and-daos/)

</div>


---

Page Title: Polkadot SDK Accounts

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/accounts.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/accounts/
- Summary: Learn about account structures, balances, and address formats in the Polkadot SDK, including how to manage lifecycle, references, and balances.
- Word Count: 4128; Token Estimate: 6308
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:f07725ffe9193db46a5e5c7ded5c607bc7ab7965f757f9ea2f8370248640d981

# Accounts

## Introduction

Accounts are essential for managing identity, transactions, and governance on the network in the Polkadot SDK. Understanding these components is critical for seamless development and operation on the network, whether you're building or interacting with Polkadot-based chains.

This page will guide you through the essential aspects of accounts, including their data structure, balance types, reference counters, and address formats. You’ll learn how accounts are managed within the runtime, how balances are categorized, and how addresses are encoded and validated. 

## Account Data Structure

Accounts are foundational to any blockchain, and the Polkadot SDK provides a flexible management system. This section explains how the Polkadot SDK defines accounts and manages their lifecycle through data structures within the runtime.

### Account

The [`Account` data type](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/type.Account.html) is a storage map within the [System pallet](https://paritytech.github.io/polkadot-sdk/master/src/frame_system/lib.rs.html) that links an account ID to its corresponding data. This structure is fundamental for mapping account-related information within the chain.

The code snippet below shows how accounts are defined:

```rs
 /// The full account information for a particular account ID.
 	#[pallet::storage]
 	#[pallet::getter(fn account)]
 	pub type Account<T: Config> = StorageMap<
 		_,
 		Blake2_128Concat,
 		T::AccountId,
 		AccountInfo<T::Nonce, T::AccountData>,
 		ValueQuery,
 	>;
```

The preceding code block defines a storage map named `Account`. The `StorageMap` is a type of on-chain storage that maps keys to values. In the `Account` map, the key is an account ID, and the value is the account's information. Here, `T` represents the generic parameter for the runtime configuration, which is defined by the pallet's configuration trait (`Config`).

The `StorageMap` consists of the following parameters:

- **`_`**: Used in macro expansion and acts as a placeholder for the storage prefix type. Tells the macro to insert the default prefix during expansion.
- **`Blake2_128Concat`**: The hashing function applied to keys in the storage map.
- **`T: :AccountId`**: Represents the key type, which corresponds to the account’s unique ID.
- **`AccountInfo<T: :Nonce, T::AccountData>`**: The value type stored in the map. For each account ID, the map stores an `AccountInfo` struct containing:

    - **`T::Nonce`**: A nonce for the account, which is incremented with each transaction to ensure transaction uniqueness.
    - **`T: :AccountData`**: Custom account data defined by the runtime configuration, which could include balances, locked funds, or other relevant information.
    
- **`ValueQuery`**: Defines how queries to the storage map behave when no value is found; returns a default value instead of `None`.

For a detailed explanation of storage maps, see the [`StorageMap`](https://paritytech.github.io/polkadot-sdk/master/frame_support/storage/types/struct.StorageMap.html) entry in the Rust docs.

### Account Info

The `AccountInfo` structure is another key element within the [System pallet](https://paritytech.github.io/polkadot-sdk/master/src/frame_system/lib.rs.html), providing more granular details about each account's state. This structure tracks vital data, such as the number of transactions and the account’s relationships with other modules.

```rs
/// Information of an account.
#[derive(Clone, Eq, PartialEq, Default, RuntimeDebug, Encode, Decode, TypeInfo, MaxEncodedLen)]
pub struct AccountInfo<Nonce, AccountData> {
	/// The number of transactions this account has sent.
	pub nonce: Nonce,
	/// The number of other modules that currently depend on this account's existence. The account
	/// cannot be reaped until this is zero.
	pub consumers: RefCount,
	/// The number of other modules that allow this account to exist. The account may not be reaped
	/// until this and `sufficients` are both zero.
	pub providers: RefCount,
	/// The number of modules that allow this account to exist for their own purposes only. The
	/// account may not be reaped until this and `providers` are both zero.
	pub sufficients: RefCount,
	/// The additional data that belongs to this account. Used to store the balance(s) in a lot of
	/// chains.
	pub data: AccountData,
}
```

The `AccountInfo` structure includes the following components:

- **`nonce`**: Tracks the number of transactions initiated by the account, which ensures transaction uniqueness and prevents replay attacks.
- **`consumers`**: Counts how many other modules or pallets rely on this account’s existence. The account cannot be removed from the chain (reaped) until this count reaches zero.
- **`providers`**: Tracks how many modules permit this account’s existence. An account can only be reaped once both `providers` and `sufficients` are zero.
- **`sufficients`**: Represents the number of modules that allow the account to exist for internal purposes, independent of any other modules.
- **`AccountData`**: A flexible data structure that can be customized in the runtime configuration, usually containing balances or other user-specific data.

This structure helps manage an account's state and prevents its premature removal while it is still referenced by other on-chain data or modules. The [`AccountInfo`](https://paritytech.github.io/polkadot-sdk/master/frame_system/struct.AccountInfo.html) structure can vary as long as it satisfies the trait bounds defined by the `AccountData` associated type in the [`frame-system::pallet::Config`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/trait.Config.html) trait.

### Account Reference Counters

Polkadot SDK uses reference counters to track an account’s dependencies across different runtime modules. These counters ensure that accounts remain active while data is associated with them.

The reference counters include:

- **`consumers`**: Prevents account removal while other pallets still rely on the account.
- **`providers`**: Ensures an account is active before other pallets store data related to it.
- **`sufficients`**: Indicates the account’s independence, ensuring it can exist even without a native token balance, such as when holding sufficient alternative assets.

#### Providers Reference Counters

The `providers` counter ensures that an account is ready to be depended upon by other runtime modules. For example, it is incremented when an account has a balance above the existential deposit, which marks the account as active.

The system requires this reference counter to be greater than zero for the `consumers` counter to be incremented, ensuring the account is stable before any dependencies are added.

#### Consumers Reference Counters

The `consumers` counter ensures that the account cannot be reaped until all references to it across the runtime have been removed. This check prevents the accidental deletion of accounts that still have active on-chain data.

It is the user’s responsibility to clear out any data from other runtime modules if they wish to remove their account and reclaim their existential deposit.

#### Sufficients Reference Counter

The `sufficients` counter tracks accounts that can exist independently without relying on a native account balance. This is useful for accounts holding other types of assets, like tokens, without needing a minimum balance in the native token.

For instance, the [Assets pallet](https://paritytech.github.io/polkadot-sdk/master/pallet_assets/index.html), may increment this counter for an account holding sufficient tokens.

#### Account Deactivation

In Polkadot SDK-based chains, an account is deactivated when its reference counters (such as `providers`, `consumers`, and `sufficient`) reach zero. These counters ensure the account remains active as long as other runtime modules or pallets reference it.

When all dependencies are cleared and the counters drop to zero, the account becomes deactivated and may be removed from the chain (reaped). This is particularly important in Polkadot SDK-based blockchains, where accounts with balances below the existential deposit threshold are pruned from storage to conserve state resources.

Each pallet that references an account has cleanup functions that decrement these counters when the pallet no longer depends on the account. Once these counters reach zero, the account is marked for deactivation.

#### Updating Counters

The Polkadot SDK provides runtime developers with various methods to manage account lifecycle events, such as deactivation or incrementing reference counters. These methods ensure that accounts cannot be reaped while still in use.

The following helper functions manage these counters:

- **`inc_consumers()`**: Increments the `consumer` reference counter for an account, signaling that another pallet depends on it.
- **`dec_consumers()`**: Decrements the `consumer` reference counter, signaling that a pallet no longer relies on the account.
- **`inc_providers()`**: Increments the `provider` reference counter, ensuring the account remains active.
- **`dec_providers()`**: Decrements the `provider` reference counter, allowing for account deactivation when no longer in use.
- **`inc_sufficients()`**: Increments the `sufficient` reference counter for accounts that hold sufficient assets.
- **`dec_sufficients()`**: Decrements the `sufficient` reference counter.

To ensure proper account cleanup and lifecycle management, a corresponding decrement should be made for each increment action.

The `System` pallet offers three query functions to assist developers in tracking account states:

- **[`can_inc_consumer()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.can_inc_consumer)**: Checks if the account can safely increment the consumer reference.
- **[`can_dec_provider()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.can_dec_provider)**: Ensures that no consumers exist before allowing the decrement of the provider counter.
- **[`is_provider_required()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.is_provider_required)**: Verifies whether the account still has any active consumer references.

This modular and flexible system of reference counters tightly controls the lifecycle of accounts in Polkadot SDK-based blockchains, preventing the accidental removal or retention of unneeded accounts. You can refer to the [System pallet Rust docs](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html) for more details.
    

## Account Balance Types

In the Polkadot ecosystem, account balances are categorized into different types based on how the funds are utilized and their availability. These balance types determine the actions that can be performed, such as transferring tokens, paying transaction fees, or participating in governance activities. Understanding these balance types helps developers manage user accounts and implement balance-dependent logic.

!!! note "A more efficient distribution of account balance types is in development"
    Soon, pallets in the Polkadot SDK will implement the [`Fungible` trait](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/tokens/fungible/index.html) (see the [tracking issue](https://github.com/paritytech/polkadot-sdk/issues/226) for more details). For example, the [`transaction-storage`](https://paritytech.github.io/polkadot-sdk/master/pallet_transaction_storage/index.html) pallet changed the implementation of the [`Currency`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/tokens/currency/index.html) trait (see the [Refactor transaction storage pallet to use fungible traits](https://github.com/paritytech/polkadot-sdk/pull/1800) PR for further details):

    ```rust
    type BalanceOf<T> = <<T as Config>::Currency as Currency<<T as frame_system::Config>::AccountId>>::Balance;
    ```
    
    To the [`Fungible`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/tokens/fungible/index.html) trait:

    ```rust
    type BalanceOf<T> = <<T as Config>::Currency as FnInspect<<T as frame_system::Config>::AccountId>>::Balance;
    ```
    
    This update will enable more efficient use of account balances, allowing the free balance to be utilized for on-chain activities such as setting proxies and managing identities.

### Balance Types

The five main balance types are:

- **Free balance**: Represents the total tokens available to the account for any on-chain activity, including staking, governance, and voting. However, it may not be fully spendable or transferrable if portions of it are locked or reserved.
- **Locked balance**: Portions of the free balance that cannot be spent or transferred because they are tied up in specific activities like [staking](https://wiki.polkadot.com/learn/learn-staking/#nominating-validators), [vesting](https://wiki.polkadot.com/learn/learn-guides-transfers/#vested-transfers-with-the-polkadot-js-ui), or participating in [governance](https://wiki.polkadot.com/learn/learn-polkadot-opengov/#voting-on-a-referendum). While the tokens remain part of the free balance, they are non-transferable for the duration of the lock.
- **Reserved balance**: Funds locked by specific system actions, such as setting up an [identity](https://wiki.polkadot.com/learn/learn-identity/), creating [proxies](https://wiki.polkadot.com/learn/learn-proxies/), or submitting [deposits for governance proposals](https://wiki.polkadot.com/learn/learn-guides-polkadot-opengov/#claiming-opengov-deposits). These tokens are not part of the free balance and cannot be spent unless they are unreserved.
- **Spendable balance**: The portion of the free balance that is available for immediate spending or transfers. It is calculated by subtracting the maximum of locked or reserved amounts from the free balance, ensuring that existential deposit limits are met.
- **Untouchable balance**: Funds that cannot be directly spent or transferred but may still be utilized for on-chain activities, such as governance participation or staking. These tokens are typically tied to certain actions or locked for a specific period.

The spendable balance is calculated as follows:

```text
spendable = free - max(locked - reserved, ED)
```

Here, `free`, `locked`, and `reserved` are defined above. The `ED` represents the [existential deposit](https://wiki.polkadot.com/learn/learn-accounts/#existential-deposit-and-reaping), the minimum balance required to keep an account active and prevent it from being reaped. You may find you can't see all balance types when looking at your account via a wallet. Wallet providers often display only spendable, locked, and reserved balances.

### Locks

Locks are applied to an account's free balance, preventing that portion from being spent or transferred. Locks are automatically placed when an account participates in specific on-chain activities, such as staking or governance. Although multiple locks may be applied simultaneously, they do not stack. Instead, the largest lock determines the total amount of locked tokens.

Locks follow these basic rules:

- If different locks apply to varying amounts, the largest lock amount takes precedence.
- If multiple locks apply to the same amount, the lock with the longest duration governs when the balance can be unlocked.

#### Locks Example

Consider an example where an account has 80 DOT locked for both staking and governance purposes like so:

- 80 DOT is staked with a 28-day lock period.
- 24 DOT is locked for governance with a 1x conviction and a 7-day lock period.
- 4 DOT is locked for governance with a 6x conviction and a 224-day lock period.

In this case, the total locked amount is 80 DOT because only the largest lock (80 DOT from staking) governs the locked balance. These 80 DOT will be released at different times based on the lock durations. In this example, the 24 DOT locked for governance will be released first since the shortest lock period is seven days. The 80 DOT stake with a 28-day lock period is released next. Now, all that remains locked is the 4 DOT for governance. After 224 days, all 80 DOT (minus the existential deposit) will be free and transferable.

![Illustration of Lock Example](/polkadot-mkdocs/images/reference/parachains/accounts/accounts-01.webp)

#### Edge Cases for Locks

In scenarios where multiple convictions and lock periods are active, the lock duration and amount are determined by the longest period and largest amount. For example, if you delegate with different convictions and attempt to undelegate during an active lock period, the lock may be extended for the full amount of tokens. For a detailed discussion on edge case lock behavior, see this [Stack Exchange post](https://substrate.stackexchange.com/questions/5067/delegating-and-undelegating-during-the-lock-period-extends-it-for-the-initial-am).

### Balance Types on Polkadot.js

Polkadot.js provides a user-friendly interface for managing and visualizing various account balances on Polkadot and Kusama networks. When interacting with Polkadot.js, you will encounter multiple balance types that are critical for understanding how your funds are distributed and restricted. This section explains how different balances are displayed in the Polkadot.js UI and what each type represents.

![](/polkadot-mkdocs/images/reference/parachains/accounts/accounts-02.webp)

The most common balance types displayed on Polkadot.js are:

- **Total balance**: The total number of tokens available in the account. This includes all tokens, whether they are transferable, locked, reserved, or vested. However, the total balance does not always reflect what can be spent immediately. In this example, the total balance is 0.6274 KSM.

- **Transferable balance**: Shows how many tokens are immediately available for transfer. It is calculated by subtracting the locked and reserved balances from the total balance. For example, if an account has a total balance of 0.6274 KSM and a transferable balance of 0.0106 KSM, only the latter amount can be sent or spent freely.

- **Vested balance**: Tokens that allocated to the account but released according to a specific schedule. Vested tokens remain locked and cannot be transferred until fully vested. For example, an account with a vested balance of 0.2500 KSM means that this amount is owned but not yet transferable.

- **Locked balance**: Tokens that are temporarily restricted from being transferred or spent. These locks typically result from participating in staking, governance, or vested transfers. In Polkadot.js, locked balances do not stack—only the largest lock is applied. For instance, if an account has 0.5500 KSM locked for governance and staking, the locked balance would display 0.5500 KSM, not the sum of all locked amounts.

- **Reserved balance**: Refers to tokens locked for specific on-chain actions, such as setting an identity, creating a proxy, or making governance deposits. Reserved tokens are not part of the free balance, but can be freed by performing certain actions. For example, removing an identity would unreserve those funds.

- **Bonded balance**: The tokens locked for staking purposes. Bonded tokens are not transferable until they are unbonded after the unbonding period.

- **Redeemable balance**: The number of tokens that have completed the unbonding period and are ready to be unlocked and transferred again. For example, if an account has a redeemable balance of 0.1000 KSM, those tokens are now available for spending.

- **Democracy balance**: Reflects the number of tokens locked for governance activities, such as voting on referenda. These tokens are locked for the duration of the governance action and are only released after the lock period ends.

By understanding these balance types and their implications, developers and users can better manage their funds and engage with on-chain activities more effectively.

## Address Formats

The SS58 address format is a core component of the Polkadot SDK that enables accounts to be uniquely identified across Polkadot-based networks. This format is a modified version of Bitcoin's Base58Check encoding, specifically designed to accommodate the multi-chain nature of the Polkadot ecosystem. SS58 encoding allows each chain to define its own set of addresses while maintaining compatibility and checksum validation for security. 

### Basic Format

SS58 addresses consist of three main components:

```text
base58encode(concat(<address-type>, <address>, <checksum>))
```

- **Address type**: A byte or set of bytes that define the network (or chain) for which the address is intended. This ensures that addresses are unique across different Polkadot SDK-based chains.
- **Address**: The public key of the account encoded as bytes.
- **Checksum**: A hash-based checksum which ensures that addresses are valid and unaltered. The checksum is derived from the concatenated address type and address components, ensuring integrity.

The encoding process transforms the concatenated components into a Base58 string, providing a compact and human-readable format that avoids easily confused characters (e.g., zero '0', capital 'O', lowercase 'l'). This encoding function ([`encode`](https://docs.rs/bs58/latest/bs58/fn.encode.html)) is implemented exactly as defined in Bitcoin and IPFS specifications, using the same alphabet as both implementations.

For more details about the SS58 address format implementation, see the [`Ss58Codec`](https://paritytech.github.io/polkadot-sdk/master/sp_core/crypto/trait.Ss58Codec.html) trait in the Rust Docs.

### Address Type

The address type defines how an address is interpreted and to which network it belongs. Polkadot SDK uses different prefixes to distinguish between various chains and address formats:

- **Address types `0-63`**: Simple addresses, commonly used for network identifiers.
- **Address types `64-127`**: Full addresses that support a wider range of network identifiers.
- **Address types `128-255`**: Reserved for future address format extensions.

For example, Polkadot’s main network uses an address type of 0, while Kusama uses 2. This ensures that addresses can be used without confusion between networks.

The address type is always encoded as part of the SS58 address, making it easy to quickly identify the network. Refer to the [SS58 registry](https://github.com/paritytech/ss58-registry) for the canonical listing of all address type identifiers and how they map to Polkadot SDK-based networks.

### Address Length

SS58 addresses can have different lengths depending on the specific format. Address lengths range from as short as 3 to 35 bytes, depending on the complexity of the address and network requirements. This flexibility allows SS58 addresses to adapt to different chains while providing a secure encoding mechanism.

| Total | Type | Raw account | Checksum |
|-------|------|-------------|----------|
| 3     | 1    | 1           | 1        |
| 4     | 1    | 2           | 1        |
| 5     | 1    | 2           | 2        |
| 6     | 1    | 4           | 1        |
| 7     | 1    | 4           | 2        |
| 8     | 1    | 4           | 3        |
| 9     | 1    | 4           | 4        |
| 10    | 1    | 8           | 1        |
| 11    | 1    | 8           | 2        |
| 12    | 1    | 8           | 3        |
| 13    | 1    | 8           | 4        |
| 14    | 1    | 8           | 5        |
| 15    | 1    | 8           | 6        |
| 16    | 1    | 8           | 7        |
| 17    | 1    | 8           | 8        |
| 35    | 1    | 32          | 2        |

SS58 addresses also support different payload sizes, allowing a flexible range of account identifiers.

### Checksum Types

A checksum is applied to validate SS58 addresses. Polkadot SDK uses a Blake2b-512 hash function to calculate the checksum, which is appended to the address before encoding. The checksum length can vary depending on the address format (e.g., 1-byte, 2-byte, or longer), providing varying levels of validation strength.

The checksum ensures that an address is not modified or corrupted, adding an extra layer of security for account management.

### Validating Addresses

SS58 addresses can be validated using the subkey command-line interface or the Polkadot.js API. These tools help ensure an address is correctly formatted and valid for the intended network. The following sections will provide an overview of how validation works with these tools.

#### Using Subkey

[Subkey](https://paritytech.github.io/polkadot-sdk/master/subkey/index.html) is a CLI tool provided by Polkadot SDK for generating and managing keys. It can inspect and validate SS58 addresses.

The `inspect` command gets a public key and an SS58 address from the provided secret URI. The basic syntax for the `subkey inspect` command is:

```bash
subkey inspect [flags] [options] uri
```

For the `uri` command-line argument, you can specify the secret seed phrase, a hex-encoded private key, or an SS58 address. If the input is a valid address, the `subkey` program displays the corresponding hex-encoded public key, account identifier, and SS58 addresses.

For example, to inspect the public keys derived from a secret seed phrase, you can run a command similar to the following:

```bash
subkey inspect "caution juice atom organ advance problem want pledge someone senior holiday very"
```

The command displays output similar to the following:

<div id="termynal" data-termynal markdown>
  <span data-ty="input"><span class="file-path"></span>subkey inspect "caution juice atom organ advance problem want pledge someone senior holiday very"</span>
  <span data-ty>Secret phrase `caution juice atom organ advance problem want pledge someone senior holiday very` is account:</span>
  <span data-ty> Secret seed: 0xc8fa03532fb22ee1f7f6908b9c02b4e72483f0dbd66e4cd456b8f34c6230b849</span>
  <span data-ty> Public key (hex): 0xd6a3105d6768e956e9e5d41050ac29843f98561410d3a47f9dd5b3b227ab8746</span>
  <span data-ty> Public key (SS58): 5Gv8YYFu8H1btvmrJy9FjjAWfb99wrhV3uhPFoNEr918utyR</span>
  <span data-ty> Account ID: 0xd6a3105d6768e956e9e5d41050ac29843f98561410d3a47f9dd5b3b227ab8746</span>
  <span data-ty> SS58 Address: 5Gv8YYFu8H1btvmrJy9FjjAWfb99wrhV3uhPFoNEr918utyR</span>
</div>
The `subkey` program assumes an address is based on a public/private key pair. If you inspect an address, the command returns the 32-byte account identifier.

However, not all addresses in Polkadot SDK-based networks are based on keys.

Depending on the command-line options you specify and the input you provided, the command output might also display the network for which the address has been encoded. For example:

```bash
subkey inspect "12bzRJfh7arnnfPPUZHeJUaE62QLEwhK48QnH9LXeK2m1iZU"
```

The command displays output similar to the following:

<div id="termynal" data-termynal markdown>
  <span data-ty="input"><span class="file-path"></span>subkey inspect "12bzRJfh7arnnfPPUZHeJUaE62QLEwhK48QnH9LXeK2m1iZU"</span>
  <span data-ty>Public Key URI `12bzRJfh7arnnfPPUZHeJUaE62QLEwhK48QnH9LXeK2m1iZU` is account:</span>
  <span data-ty> Network ID/Version: polkadot</span>
  <span data-ty> Public key (hex): 0x46ebddef8cd9bb167dc30878d7113b7e168e6f0646beffd77d69d39bad76b47a</span>
  <span data-ty> Account ID: 0x46ebddef8cd9bb167dc30878d7113b7e168e6f0646beffd77d69d39bad76b47a</span>
  <span data-ty> Public key (SS58): 12bzRJfh7arnnfPPUZHeJUaE62QLEwhK48QnH9LXeK2m1iZU</span>
  <span data-ty> SS58 Address: 12bzRJfh7arnnfPPUZHeJUaE62QLEwhK48QnH9LXeK2m1iZU</span>
</div>
#### Using Polkadot.js API

To verify an address in JavaScript or TypeScript projects, you can use the functions built into the [Polkadot.js API](https://polkadot.js.org/docs/). For example:

```js
// Import Polkadot.js API dependencies
const { decodeAddress, encodeAddress } = require('@polkadot/keyring');
const { hexToU8a, isHex } = require('@polkadot/util');

// Specify an address to test.
const address = 'INSERT_ADDRESS_TO_TEST';

// Check address
const isValidSubstrateAddress = () => {
  try {
    encodeAddress(isHex(address) ? hexToU8a(address) : decodeAddress(address));

    return true;
  } catch (error)
};

// Query result
const isValid = isValidSubstrateAddress();
console.log(isValid);
```

If the function returns `true`, the specified address is a valid address.

#### Other SS58 Implementations

Support for encoding and decoding Polkadot SDK SS58 addresses has been implemented in several other languages and libraries.

- **Crystal**: [`wyhaines/base58.cr`](https://github.com/wyhaines/base58.cr)
- **Go**: [`itering/subscan-plugin`](https://github.com/itering/subscan-plugin)
- **Python**: [`polkascan/py-scale-codec`](https://github.com/polkascan/py-scale-codec)
- **TypeScript**: [`subsquid/squid-sdk`](https://github.com/subsquid/squid-sdk)


---

Page Title: Randomness

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/randomness.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/randomness/
- Summary: Explore the importance of randomness in PoS blockchains, focusing on Polkadot’s VRF-based approach to ensure fairness and security in validator selection.
- Word Count: 993; Token Estimate: 1348
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:3a9d58ef2aabdba3c8e83211aa1626508bc7dc7c2a681f4b4f0dceee16714100

# Randomness

## Introduction

Randomness is crucial in Proof of Stake (PoS) blockchains to ensure a fair and unpredictable distribution of validator duties. However, computers are inherently deterministic, meaning the same input always produces the same output. What we typically refer to as "random" numbers on a computer are actually pseudo-random. These numbers rely on an initial "seed," which can come from external sources like [atmospheric noise](https://www.random.org/randomness/), [heart rates](https://mdpi.altmetric.com/details/47574324), or even [lava lamps](https://en.wikipedia.org/wiki/Lavarand). While this may seem random, given the same "seed," the same sequence of numbers will always be generated.

In a global blockchain network, relying on real-world entropy for randomness isn’t feasible because these inputs vary by time and location. If nodes use different inputs, blockchains can fork. Hence, real-world randomness isn't suitable for use as a seed in blockchain systems.

Currently, two primary methods for generating randomness in blockchains are used: [`RANDAO`](#randao) and [`VRF`](#vrf) (Verifiable Random Function). Polkadot adopts the `VRF` approach for its randomness.

## VRF

A Verifiable Random Function (VRF) is a cryptographic function that generates a random number and proof that ensures the submitter produced the number. This proof allows anyone to verify the validity of the random number.

Polkadot's VRF is similar to the one used in [**Ouroboros Praos**](https://eprint.iacr.org/2017/573.pdf), which secures randomness for block production in systems like [BABE](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/#block-production-babe) (Polkadot’s block production mechanism). 

The key difference is that Polkadot's VRF doesn’t rely on a central clock—avoiding the issue of whose clock to trust. Instead, it uses its own past results and slot numbers to simulate time and determine future outcomes.

### How VRF Works

Slots on Polkadot are discrete units of time, each lasting six seconds, and can potentially hold a block. Multiple slots form an epoch, with 2400 slots making up one four-hour epoch.

In each slot, validators execute a "die roll" using a VRF. The VRF uses three inputs:

1. A "secret key," unique to each validator, is used for the die roll.
2. An epoch randomness value, derived from the hash of VRF outputs from blocks two epochs ago (N-2), so past randomness influences the current epoch (N).
3. The current slot number.

This process helps maintain fair randomness across the network.

Here is a graphical representation:

![](/polkadot-mkdocs/images/reference/parachains/randomness/randomness-01.webp)

The VRF produces two outputs: a result (the random number) and a proof (verifying that the number was generated correctly).

The result is checked by the validator against a protocol threshold. If it's below the threshold, the validator becomes a candidate for block production in that slot. 

The validator then attempts to create a block, submitting it along with the `PROOF` and `RESULT`.

So, VRF can be expressed like:

`(RESULT, PROOF) = VRF(SECRET, EPOCH_RANDOMNESS_VALUE, CURRENT_SLOT_NUMBER)`

Put simply, performing a "VRF roll" generates a random number along with proof that the number was genuinely produced and not arbitrarily chosen.

After executing the VRF, the `RESULT` is compared to a protocol-defined `THRESHOLD`. If the `RESULT` is below the `THRESHOLD`, the validator becomes a valid candidate to propose a block for that slot. Otherwise, the validator skips the slot.

As a result, there may be multiple validators eligible to propose a block for a slot. In this case, the block accepted by other nodes will prevail, provided it is on the chain with the latest finalized block as determined by the GRANDPA finality gadget. It's also possible for no block producers to be available for a slot, in which case the AURA consensus takes over. AURA is a fallback mechanism that randomly selects a validator to produce a block, running in parallel with BABE and only stepping in when no block producers exist for a slot. Otherwise, it remains inactive.

Because validators roll independently, no block candidates may appear in some slots if all roll numbers are above the threshold. 

To verify resolution of this issue and that Polkadot block times remain near constant-time, see the [PoS Consensus](/polkadot-mkdocs/reference/polkadot-hub/consensus-and-security/pos-consensus/) page of this documentation.

## RANDAO

An alternative on-chain randomness method is Ethereum's RANDAO, where validators perform thousands of hashes on a seed and publish the final hash during a round. The collective input from all validators forms the random number, and as long as one honest validator participates, the randomness is secure.

To enhance security, RANDAO can optionally be combined with a Verifiable Delay Function (VDF), ensuring that randomness can't be predicted or manipulated during computation.

For more information about RANDAO, see the [Randomness - RANDAO](https://eth2book.info/capella/part2/building_blocks/randomness/) section of the Upgrading Ethereum documentation.

## VDFs

Verifiable Delay Functions (VDFs) are time-bound computations that, even on parallel computers, take a set amount of time to complete. 

They produce a unique result that can be quickly verified publicly. When combined with RANDAO, feeding RANDAO's output into a VDF introduces a delay that nullifies an attacker's chance to influence the randomness.

However, VDF likely requires specialized ASIC devices to run separately from standard nodes.

!!!warning 
    While only one is needed to secure the system, and they will be open-source and inexpensive, running VDF devices involves significant costs without direct incentives, adding friction for blockchain users.

## Additional Resources

For more information about the reasoning for choices made along with proofs, see Polkadot's research on blockchain randomness and sortition in the [Block production](https://research.web3.foundation/Polkadot/protocols/block-production) entry of the Polkadot Wiki. 

For a discussion with Web3 Foundation researchers about when and under what conditions Polkadot's randomness can be utilized, see the [Discussion on Randomness used in Polkadot](https://github.com/use-ink/ink/issues/57) issue on GitHub.


---

Page Title: Register a Foreign Asset

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/token-operations/register-foreign-asset.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/token-operations/register-foreign-asset/
- Summary: Learn step-by-step how to register a foreign asset on Polkadot Hub and the origin parachain to enable cross-chain token transfers.
- Word Count: 1414; Token Estimate: 2179
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:be9b279075b94c86cd55a8a90a2238d06ebe05e7df51508a1064df73202cc9f7

# Register a Foreign Asset on Polkadot Hub

## Introduction

As outlined in the [Assets on Polkadot Hub Overview](/polkadot-mkdocs/reference/polkadot-hub/assets/), Polkadot Hub supports two categories of assets: local and foreign. Local assets are created on Polkadot Hub and are identified by integer IDs. On the other hand, foreign assets, which originate outside of Polkadot Hub, are recognized by [Multilocations](https://github.com/polkadot-fellows/xcm-format?tab=readme-ov-file#7-universal-consensus-location-identifiers).

When registering a foreign asset on Polkadot Hub, it's essential to notice that the process involves communication between two parachains. The Polkadot Hub chain will be the destination of the foreign asset, while the source parachain will be the origin of the asset. The communication between the two parachains is facilitated by the [Cross-Chain Message Passing (XCMP)](/polkadot-mkdocs/parachains/interoperability/get-started/) protocol.

This guide will take you through the process of registering a foreign asset on Polkadot Hub.

## Prerequisites

Polkadot Hub is a system parachain on a relay chain, such as [Polkadot](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpolkadot.api.onfinality.io%2Fpublic-ws#/explorer) or [Kusama](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama.api.onfinality.io%2Fpublic-ws#/explorer). To interact with these parachains, you can use the [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) interface for:

- [Polkadot Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fasset-hub-polkadot-rpc.dwellir.com#/explorer)
- [Kusama Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fsys.ibp.network%2Fstatemine#/explorer)

For testing purposes, you can also interact with the TestNet Polkadot Hub instance:

- [TestNet Polkadot Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpas-rpc.stakeworld.io%2Fassethub#/explorer)

Before you start, ensure that you have: 

- Access to the Polkadot.js Apps interface, and you are connected to the desired chain.
- A parachain that supports the XCMP protocol to interact with the Polkadot Hub parachain.
- A funded wallet to pay for the transaction fees and subsequent registration of the foreign asset.

This guide will use Polkadot, its local Polkadot Hub instance, and the [Astar](https://astar.network/) parachain (`ID` 2006), as stated in the [Test Environment Setup](#test-environment-setup) section. However, the process is the same for other relay chains and their respective Polkadot Hub parachain, regardless of the network you are using and the parachain owner of the foreign asset.

## Register a Foreign Asset

### Polkadot Hub

To register a foreign asset on Polkadot Hub, follow these steps:

1. Open the [Polkadot.js Apps](https://polkadot.js.org/apps/) interface and connect to Polkadot Hub using the network selector in the top left corner.

      - Testing foreign asset registration is recommended on TestNet before proceeding to MainNet. If you haven't set up a local testing environment yet, consult the [Environment setup](#test-environment-setup) guide. After setting up, connect to the Local Node (Chopsticks) at `ws://127.0.0.1:8000`.
      - For live network operations, connect to Polkadot Hub. You can choose either Polkadot or Kusama Asset Hub from the dropdown menu, selecting your preferred RPC provider.

2. Navigate to the **Extrinsics** page:

      1. Click on the **Developer** tab from the top navigation bar.
      2. Select **Extrinsics** from the dropdown.

    ![Access to Developer Extrinsics section](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-01.webp)

3. Select the Foreign Assets pallet:

      3. Select the **`foreignAssets`** pallet from the dropdown list.
      4. Choose the **`create`** extrinsic.

    ![Select the Foreign Asset pallet](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-02.webp)

3. Fill out the required fields and click on the copy icon to copy the **encoded call data** to your clipboard. The fields to be filled are:

    - **id**: As this is a foreign asset, the ID will be represented by a Multilocation that reflects its origin. For this case, the Multilocation of the asset will be from the source parachain perspective.
  
        ```javascript
        { parents: 1, interior: { X1: [{ Parachain: 2006 }] } }
        ```

    - **admin**: Refers to the account that will be the admin of this asset. This account will be able to manage the asset, including updating its metadata. As the registered asset corresponds to a native asset of the source parachain, the admin account should be the sovereign account of the source parachain.
      
        The sovereign account can be obtained through [Substrate Utilities](https://www.shawntabrizi.com/substrate-js-utilities/).

        Ensure that **Sibling** is selected and that the **Para ID** corresponds to the source parachain. In this case, since the guide follows the test setup stated in the [Test Environment Setup](#test-environment-setup) section, the **Para ID** is `2006`.

        ![Get parachain sovereign account](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-03.webp)


    - **`minBalance`**: The minimum balance required to hold this asset.

    ![Fill out the required fields](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-04.webp)

    !!! tip 
        If you need an example of the encoded call data, you can copy the following:
        ```
        0x3500010100591f007369626cd6070000000000000000000000000000000000000000000000000000a0860100000000000000000000000000
        ```

### Source Parachain

With the encoded call data you generated in the previous section, you can take the following steps on the source parachain to complete the asset registration process:

1. Navigate to the **Developer > Extrinsics** section.
2. Create the extrinsic to register the foreign asset through XCM:

      1. Paste the **encoded call data** copied in the previous step.
      2. Click the **Submit Transaction** button.

    ![Register foreign asset through XCM](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-05.webp)

    This XCM call involves withdrawing DOT from the sibling account of the parachain, using it to initiate an execution. The transaction will be carried out with XCM as the origin kind, and will be a hex-encoded call to create a foreign asset on Polkadot Hub for the specified parachain asset multilocation. Any surplus will be refunded, and the asset will be deposited into the sibling account.

    !!! warning
        Note that the sovereign account on Polkadot Hub must have a sufficient balance to cover the XCM `BuyExecution` instruction. If the account does not have enough balance, the transaction will fail.

    If you want to have the whole XCM call ready to be copied, go to the **Developer > Extrinsics > Decode** section and paste the following hex-encoded call data:

    ```text
    0x6300330003010100a10f030c000400010000070010a5d4e81300010000070010a5d4e80006030700b4f13501419ce03500010100591f007369626cd607000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ```

    Be sure to replace the encoded call data with the one you copied in the previous step.

After the transaction is successfully executed, the foreign asset will be registered on Polkadot Hub. 

## Asset Registration Verification

To confirm that a foreign asset has been successfully accepted and registered on Polkadot Hub, you can navigate to the **Network > Explorer** section of the Polkadot.js Apps interface for Polkadot Asset Hub. You should be able to see an event that includes the following details:

![Asset registration event](/polkadot-mkdocs/images/chain-interactions/token-operations/register-foreign-asset/register-a-foreign-asset-06.webp)

In the image above, the **success** field indicates whether the asset registration was successful.

## Test Environment Setup

To test the foreign asset registration process before deploying it on a live network, you can set up a local parachain environment. This guide uses Chopsticks to simulate that process. For more information on using Chopsticks, please refer to the [Chopsticks documentation](/polkadot-mkdocs/parachains/testing/fork-a-parachain/).

To set up a test environment, run the following command:

```bash
npx @acala-network/chopsticks xcm \
--r polkadot \
--p polkadot-asset-hub \
--p astar
```

The preceding command will create a lazy fork of Polkadot as the relay chain, its Polkadot Hub instance, and the Astar parachain. The `xcm` parameter enables communication through the XCMP protocol between the relay chain and the parachains, allowing the registration of foreign assets on Polkadot Hub. For further information on the chopsticks usage of the XCMP protocol, refer to the [Replay and Dry Run XCMs using Chopsticks](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/debug-and-preview-xcms/) guide.

After executing the command, the terminal will display output indicating the Polkadot relay chain, Polkadot Hub, and the Astar parachain are running locally and connected through XCM. You can access them individually via the Polkadot.js Apps interface.

- [Polkadot Relay Chain](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Flocalhost%3A8002#/explorer)
- [Polkadot Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Flocalhost%3A8000#/explorer)
- [Astar Parachain](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Flocalhost%3A8001#/explorer)


---

Page Title: Register a Local Asset

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/token-operations/register-local-asset.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/token-operations/register-local-asset/
- Summary: Learn how to register a local asset on Polkadot Hub, including prerequisites, deposits, and step-by-step instructions using Polkadot.js Apps.
- Word Count: 1008; Token Estimate: 1516
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:59e606a0ce17ef1e38232956242b7bd0ec97489e00a13ff61126ceab693e3121

# Register a Local Asset on Polkadot Hub

## Introduction

As detailed in the [Assets on Polkadot Hub Overview](/polkadot-mkdocs/reference/polkadot-hub/assets/) page, Polkadot Hub accommodates two types of assets: local and foreign. Local assets are those that were created in Polkadot Hub and are identifiable by an integer ID. On the other hand, foreign assets originate from a sibling parachain and are identified by a Multilocation.

This guide will take you through the steps of registering a local asset on Polkadot Hub.

## Prerequisites

Before you begin, ensure you have access to the [Polkadot.js Apps](https://polkadot.js.org/apps/) interface and a funded wallet with DOT or KSM.

- For Polkadot Hub, you would need a deposit of 10 DOT and around 0.201 DOT for the metadata.
- For Kusama Hub, the deposit is 0.1 KSM and around 0.000669 KSM for the metadata.

You need to ensure that your Polkadot Hub account balance is a bit more than the sum of those two deposits, which should seamlessly account for the required deposits and transaction fees.

## Register a Local Asset

To register a local asset on Polkadot Hub, follow these steps:

1. Open the [Polkadot.js Apps](https://polkadot.js.org/apps/) interface and connect to the Polkadot Asset Hub using the network selector in the top left corner.

      - You may prefer to test local asset registration on TestNet before registering the asset on a MainNet hub. If you still need to set up a local testing environment, review the [Environment setup](#test-setup-environment) section for instructions. Once the local environment is set up, connect to the Local Node (Chopsticks) available on `ws://127.0.0.1:8000`.
      - For the live network, connect to the **Asset Hub** parachain. Either Polkadot or Kusama Asset Hub can be selected from the dropdown list, choosing the desired RPC provider.

2. Click on the **Network** tab on the top navigation bar and select **Assets** from the dropdown list.

      ![Access to Polkadot Hub through Polkadot.JS](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-01.webp)

3. Now, you need to examine all the registered asset IDs. This step is crucial to ensure that the asset ID you are about to register is unique. Asset IDs are displayed in the **assets** column.

      ![Asset IDs on Polkadot Hub](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-02.webp)

4. Once you have confirmed that the asset ID is unique, click on the **Create** button on the top right corner of the page.

      ![Create a new asset](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-03.webp)

5. Fill in the required fields in the **Create Asset** form:

    1. **creator account**: The account to be used for creating this asset and setting up the initial metadata.
    2. **asset name**: The descriptive name of the asset you are registering.
    3. **asset symbol**: The symbol that will be used to represent the asset.
    4. **asset decimals**: The number of decimal places for this token, with a maximum of 20 allowed through the user interface.
    5. **minimum balance**: The minimum balance for the asset. This is specified in the units and decimals as requested.
    6. **asset ID**: The selected id for the asset. This should not match an already-existing asset id.
    7. Click on the **Next** button.
 
    ![Create Asset Form](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-04.webp)

6. Choose the accounts for the roles listed below:

    1. **admin account**: The account designated for continuous administration of the token.
    2. **issuer account**: The account that will be used for issuing this token.
    3. **freezer account**: The account that will be used for performing token freezing operations.
    4. Click on the **Create** button.

    ![Admin, Issuer, Freezer accounts](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-05.webp)

7. Click on the **Sign and Submit** button to complete the asset registration process.

    ![Sign and Submit](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-06.webp)

## Verify Asset Registration

After completing these steps, the asset will be successfully registered. You can now view your asset listed on the [**Assets**](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fasset-hub-polkadot-rpc.dwellir.com#/assets) section of the Polkadot.js Apps interface.

![Asset listed on Polkadot.js Apps](/polkadot-mkdocs/images/chain-interactions/token-operations/register-local-asset/register-a-local-asset-07.webp)

!!! tip
    Take into consideration that the **Assets** section’s link may differ depending on the network you are using. For the local environment, enter `ws://127.0.0.1:8000` into the **Custom Endpoint** field.

In this way, you have successfully registered a local asset on the Polkadot Hub.

For an in-depth explanation about Polkadot Hub and its features, see the [Polkadot Hub](/polkadot-mkdocs/chain-interactions/token-operations/convert-assets/) entry in the Polkadot Wiki.

## Test Setup Environment

You can set up a local parachain environment to test the asset registration process before deploying it on the live network. This guide uses Chopsticks to simulate that process. For further information on chopsticks usage, refer to the [Chopsticks](/polkadot-mkdocs/parachains/testing/fork-a-parachain/) documentation.

To set up a test environment, execute the following command:

```bash
npx @acala-network/chopsticks \
--config=https://raw.githubusercontent.com/AcalaNetwork/chopsticks/master/configs/polkadot-asset-hub.yml
```

The above command will spawn a lazy fork of Polkadot Hub with the latest block data from the network. If you need to test Kusama Hub, replace `polkadot-asset-hub.yml` with `kusama-asset-hub.yml` in the command.

A Polkadot Hub instance is now running locally, and you can proceed with the asset registration process. Note that the local registration process does not differ from the live network process. Once you have a successful TestNet transaction, you can use the same steps to register the asset on MainNet.


---

Page Title: Replay and Dry Run XCMs

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/debug-and-preview-xcms.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/debug-and-preview-xcms/
- Summary: Replay and dry-run XCMs using Chopsticks with full logging enabled. Diagnose issues, trace message flow, and debug complex cross-chain interactions.
- Word Count: 2039; Token Estimate: 4075
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:aa6328e370f8a7f6d8d16357a3997ba181d0a37bdfed28dbac6a7bb299d108ba

# Replay and Dry Run XCMs Using Chopsticks

## Introduction

This guide demonstrates how to replay and dry-run XCMs using [Chopsticks](/polkadot-mkdocs/reference/tools/chopsticks/), a powerful tool for forking live Polkadot SDK-based chains in your local environment. These techniques are essential for debugging cross-chain message failures, tracing execution across relay chains and parachains, analyzing weight usage and error types, and safely simulating XCMs without committing state changes.

By following this guide, you will learn how to set up a local fork, capture and replay real XCMs, and use dry-run features to diagnose and resolve complex cross-chain issues.

## Prerequisites

Before you begin, make sure you have:

- [Chopsticks](/polkadot-mkdocs/reference/tools/chopsticks/) installed
- Access to the endpoint or genesis file of the parachain you want to fork
- The block number or hash where the XCM was sent
- (Optional) A Chopsticks config file for repeated setups

## Set Up Your Project

Start by creating a dedicated workspace for your XCM replay and dry-run experiments:

1. Create a new directory and navigate into it:

    ```bash
    mkdir -p replay-xcm-tests
    cd replay-xcm-tests
    ```

2. Initialize a new Node project:

    ```bash
    npm init -y && npm pkg set type=module
    ```

3. Install Chopsticks globally (recommended to avoid conflicts with local installs):

    ```bash
    npm install -g @acala-network/chopsticks@latest
    ```

4. Install TypeScript and related tooling for local development:

    ```bash
    npm install --save-dev typescript @types/node tsx
    ```

5. Install the required Polkadot packages:

    ```bash
    npm install polkadot-api@1.23.3 @polkadot-labs/hdkd@0.0.27 @polkadot-labs/hdkd-helpers@0.0.28
    ```

6. Initialize the TypeScript config:

    ```bash
    npx tsc --init
    ```

## Capture the XCM to Replay

To replay a specific XCM, identify:

- The source and destination chains involved
- The block number or height where the XCM was sent
- Optionally, the call payload (if you plan to simulate it manually via development commands)

You can use Polkadot.js Apps, [papi console](https://dev.papi.how/), or indexers such as [Subscan](https://polkadot.subscan.io/xcm_dashboard) to locate and inspect the original XCM execution.

## Fork the Relevant Chains

Use Chopsticks to fork the required chains at the appropriate block heights.

### Set the Block Numbers

Create or edit a `.env` file with the block heights for each chain. These should be just before the XCM is sent to allow a full replay:

```text title=".env"
POLKADOT_BLOCK_NUMBER=26481107
POLKADOT_HUB_BLOCK_NUMBER=9079591
ACALA_BLOCK_NUMBER=8826385
```

### Enable Logging and Wasm Override

Full execution logs only work if the runtime was compiled with logging enabled. Most live chains are built using the `production` profile, which disables logs. To enable logging, you'll need to override the Wasm with a locally built `release` or `debug` version. The `release` profile is faster to load in Chopsticks.

1. Clone the `polkadot-fellows/runtimes` repository:

    ```bash
    git clone git@github.com:polkadot-fellows/runtimes.git
    ```

2. Build the Polkadot Hub runtime:

    ```bash
    cd runtimes
    # Build with the `debug` profile (default): 
    # cargo build -p asset-hub-polkadot-runtime

    # Build with the `release` profile (faster to load in Chopsticks)
    cargo build --release -p asset-hub-polkadot-runtime
    ```

3. Copy the compiled Wasm to your working directory:

    ```bash
    # Assuming you're still in the `runtimes` directory
    mkdir -p ../wasms  # or your <replay-xcm-tests>/wasms path

    # Copy the compiled Wasm to your working directory:

    # If built with the `debug` profile:
    # cp target/debug/wbuild/asset-hub-polkadot-runtime/asset_hub_polkadot_runtime.wasm ../wasms

    # If built with the `release` profile:
    cp target/release/wbuild/asset-hub-polkadot-runtime/asset_hub_polkadot_runtime.compact.compressed.wasm ../wasms
    ```

4. Download and modify a config file:

    ```bash
    # Still in the `runtimes` directory
    cd .. # Return to your replay-xcm-tests root
    mkdir -p configs
    wget https://raw.githubusercontent.com/AcalaNetwork/chopsticks/master/configs/polkadot-asset-hub.yml -O configs/polkadot-asset-hub-override.yaml
    ```

5. Edit `configs/polkadot-asset-hub-override.yaml` to include:

    ```yaml title="configs/polkadot-asset-hub-override.yaml"
    ...
    runtime-log-level: 5
    # wasm-override: wasms/asset_hub_polkadot_runtime.wasm                     # Uncomment if using the `debug` build
    wasm-override: wasms/asset_hub_polkadot_runtime.compact.compressed.wasm    # Use this if you built with `release`
    ...
    ```

6. Start the forked chains using your custom config:

    ```bash
    npx @acala-network/chopsticks xcm \
    -r polkadot \
    -p configs/polkadot-asset-hub-override.yaml \
    -p acala
    ```

    This command starts the relay chain and parachains locally with full runtime execution logs enabled. Once the chains are running, you should see output indicating that the following RPC endpoints are available:

    - Polkadot Hub RPC on `http://localhost:8000`
    - Acala RPC on `http://localhost:8001`
    - Polkadot RPC on `http://localhost:8002`

    You'll also see runtime logs such as:

    <div class="termynal" data-termynal>
      <span data-ty="input">npx @acala-network/chopsticks xcm \ -r polkadot \ -p configs/polkadot-asset-hub-override.yaml \ -p acala</span>
      <span data-ty>[09:29:14.988] INFO: Polkadot Asset Hub RPC listening on http://[::]:8000 and ws://[::]:8000</span>
      <span data-ty>[09:29:14.988] INFO: Loading config file https://raw.githubusercontent.com/AcalaNetwork/chopsticks/master/configs/acala.yml</span>
      <span data-ty>[09:29:15.984] INFO: Acala RPC listening on http://[::]:8001 and ws://[::]:8001</span>
      <span data-ty>[09:29:15.990] INFO (xcm): Connected parachains [1000,2000]</span>
      <span data-ty>[09:29:15.990] INFO: Loading config file https://raw.githubusercontent.com/AcalaNetwork/chopsticks/master/configs/polkadot.yml</span>
      <span data-ty>[09:29:16.927] INFO: Polkadot RPC listening on http://[::]:8002 and ws://[::]:8002</span>
      <span data-ty>[09:29:16.984] INFO (xcm): Connected relaychain 'Polkadot' with parachain 'Polkadot Asset Hub'</span>
      <span data-ty>[09:29:17.028] INFO (xcm): Connected relaychain 'Polkadot' with parachain 'Acala'</span>
    </div>
## Identify and Extract the XCM

To replay an XCM, you'll first need to identify the exact extrinsic that triggered it. This example uses block 9079592 on Polkadot Hub.

1. Find and open the block on Subscan to inspect its extrinsics and events. In this case, the block is [9079592](https://assethub-polkadot.subscan.io/block/9079592).

2. Copy the block hash. Look for the block hash at the top of the page. For block 9079592, the hash is:

    ```bash title="Block Hash"
    0xeb5a5737d47367dc1c02b978232283cdb096eb7e51d2eb22366a106a011347f6
    ```

3. Explore and view the block in [Polkadot.js Apps](https://polkadot.js.org/apps) using this direct link: [Block Hash Explorer](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpolkadot-asset-hub-rpc.polkadot.io#/explorer/query/0xeb5a5737d47367dc1c02b978232283cdb096eb7e51d2eb22366a106a011347f6).

4. Locate and decode the XCM extrinsic. Once you've found the extrinsic (e.g., 9079592-2), extract and decode its call data. For example, the call data is:
   
    ```bash title="Call Data"
    0xad028400fc39fcf04a8071b7409823b7c82427ce67910c6ed80aa0e5093aff234624c820016a30461702adc48213e5c9ee4d15c5a481c578cb5cbc935f0bd11fe8aee489082a745ffbbe94282f91b67daa6cb44920d77c30849c1d25f5f6c3e59015a3e383440055040000011f0803010100411f0300010100fc39fcf04a8071b7409823b7c82427ce67910c6ed80aa0e5093aff234624c8200304000002043205011f0092e81d790000000000
    ```

5. From the decoded view, copy the **hex-encoded call** (e.g. `0x1f08...0000`). You'll pass this into `api.txFromCallData(...)` to replay the XCM locally.

## Replay the XCM

Once your project is set up, you're ready to replay the XCM locally.

This is useful for:

- Diagnosing execution failures or weight limits
- Inspecting all emitted events
- Verifying behavior before submitting a real transaction

### Add the Polkadot Hub Descriptor

Add the Polkadot Hub descriptor to use type-safe APIs with PAPI:

```bash
npx papi add polkadotHub -w ws://localhost:8000
```

!!! note
    The script assumes Polkadot Hub is served on `ws://localhost:8000`. If you're using a different port or configuration, update the WebSocket endpoint in the script or descriptor.

### Create a Replay Script

Create a file named `replay-xcm.ts` and add the following code to it:

```ts title="replay-xcm.ts"
import { Binary, createClient, Transaction } from 'polkadot-api';
import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
import { getPolkadotSigner } from 'polkadot-api/signer';
import { getWsProvider } from 'polkadot-api/ws-provider/web';
import { polkadotHub } from '@polkadot-api/descriptors';
import { sr25519CreateDerive } from '@polkadot-labs/hdkd';
import {
  DEV_PHRASE,
  entropyToMiniSecret,
  mnemonicToEntropy,
} from '@polkadot-labs/hdkd-helpers';

const toHuman = (_key: any, value: any) => {
  if (typeof value === 'bigint')

  if (value && typeof value === 'object' && typeof value.asHex === 'function')

  return value;
};

function getSigner()

async function main(): ${ev.block.hash}`,
        );

        if (!ev.ok) → ${modErr.value?.type}`,
            );
          } else {
            console.error(
              '❌ Dispatch error:',
              JSON.stringify(dispatchError, toHuman, 2),
            );
          }
        }

        for (const event of ev.events)

        console.log('✅ Process completed, exiting...');
        subscription.unsubscribe();
        resolve();
      }
    });
  });

  client.destroy();
}

main().catch(console.error);
```

### Execute the Replay Script

Ensure Chopsticks is running and serving a chain that includes `pallet-xcm`, such as a Polkadot Hub fork. Run the script:

```bash
npx tsx replay-xcm.ts
```

### Expected Output

You should see output similar to:

<div class="termynal" data-termynal>
  <span data-ty="input">npx tsx replay-xcm.ts</span>
  <pre data-ty>
executing xcm: {
  "type": "polkadotxcm",
  "value": {
    "type": "limited_reserve_transfer_assets",
    "value": {
      "dest": { "parents": 0, "interior": { "X1": [{ "Parachain": 2006 }] } },
      "beneficiary": { "parents": 0, "interior": { "X1": [{ "AccountId32": { "network": null, "id": "0x..." } }] } },
      "assets": [{ "id": { "Concrete": { "parents": 0, "interior": "Here" } }, "fun": { "Fungible": 120000000000 } }],
      "fee_asset_item": 0,
      "weight_limit": { "type": "Unlimited" }
    }
  }
}
  </pre>
  <span data-ty>📦 Included in block #9079592: 0x227a11c64f6051ba2e090a13abd17e5f7581640a80f6c03fc2d43fac66ab7949</span>
  <span data-ty>📣 Event: Balances { "type": "Upgraded", "value": { ... } }</span>
  <span data-ty>📣 Event: Balances { "type": "Withdraw", "value": { ... } }</span>
  <span data-ty>📣 Event: Assets { "type": "Transferred", "value": { ... } }</span>
  <span data-ty>📣 Event: PolkadotXcm { "type": "Attempted", "value": { ... } }</span>
  <span data-ty>📣 Event: Balances { "type": "Burned", "value": { ... } }</span>
  <span data-ty>📣 Event: Balances { "type": "Minted", "value": { ... } }</span>
  <span data-ty>📣 Event: PolkadotXcm { "type": "FeesPaid", "value": { ... } }</span>
  <span data-ty>📣 Event: XcmpQueue { "type": "XcmpMessageSent", "value": { ... } }</span>
  <span data-ty>📣 Event: PolkadotXcm { "type": "Sent", "value": { ... } }</span>
  <span data-ty>📣 Event: Balances { "type": "Deposit", "value": { ... } }</span>
  <span data-ty>📣 Event: TransactionPayment { "type": "TransactionFeePaid", "value": { ... } }</span>
  <span data-ty>📣 Event: System { "type": "ExtrinsicSuccess", "value": { ... } }</span>
  <span data-ty>✅ Process completed, exiting...</span>
</div>
## Dry Run the XCM

To simulate the XCM without actually sending it, you can use the `dry_run_call` method. This lets you check whether the XCM would succeed without modifying any state.

### Create a Dry Run Script

Assuming you have the `tx` transaction from the previous step, create a new script named `dry-run-call.ts` and add the following code to it:

```ts title="dry-run-call.ts"
import { Binary, createClient, Enum } from 'polkadot-api';
import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
import { getWsProvider } from 'polkadot-api/ws-provider/web';
import { polkadotHub } from '@polkadot-api/descriptors';
import { sr25519CreateDerive } from '@polkadot-labs/hdkd';
import {
  DEV_PHRASE,
  entropyToMiniSecret,
  mnemonicToEntropy,
  ss58Address,
} from '@polkadot-labs/hdkd-helpers';

const XCM_VERSION = 5;

async function main());

  client.destroy();
}

main().catch(console.error);
```

Ensure your local Chopsticks fork is running and the ports match those used in the script.

### Execute the Dry Run Script

Run the script:

```bash
npx tsx dry-run-call.ts
```

If successful, the dry run confirms that the XCM would execute correctly:

<div class="termynal" data-termynal>
  <span data-ty="input">npx tsx dry-run-call.ts</span>
  <pre data-ty>
execution_result: {
  "success": true,
  "value": {
    "post_info": { "actual_weight": 123456, "pays_fee": "Yes" },
    "result": "Ok"
  }
}
emitted_events: [ { "section": "Balances", "method": "Transfer", "data": { "from": "0x...", "to": "0x...", "amount": 1000000000 } } ]
local_xcm: { "type": "SomeType", "value": { ... } }
forwarded_xcms: []
  </pre>
  <span data-ty>✅ Dry run succeeded</span>
  <span data-ty>✅ Process completed, exiting...</span>
</div>
If it fails, you'll receive detailed error information:

<div class="termynal" data-termynal>
  <span data-ty="input">npx tsx dry-run-call.ts</span>
  <pre data-ty>
execution_result: {
  "success": false,
  "value": {
    "post_info": { "actual_weight": 123456, "pays_fee": "Yes" },
    "error": {
      "type": "Module",
      "value": {
        "type": "PolkadotXcm",
        "value": { "type": "LocalExecutionIncomplete", "value": null }
      }
    }
  }
}
  </pre>
  <span data-ty>❌ Dry run failed: LocalExecutionIncomplete</span>
  <span data-ty>✅ Process completed, exiting...</span>
</div>
For more information, see:

- [Dry Run Call](https://paritytech.github.io/polkadot-sdk/master/xcm_runtime_apis/dry_run/trait.DryRunApi.html#method.dry_run_call) to simulate a full extrinsic.
- [Dry Run XCM](https://paritytech.github.io/polkadot-sdk/master/xcm_runtime_apis/dry_run/trait.DryRunApi.html#method.dry_run_xcm) to simulate a raw XCM.

## Review and Debug

Replaying XCMs with full logging provides fine-grained control and visibility into cross-chain message behavior. Chopsticks makes this possible in a safe, local environment, empowering developers to debug complex message flows, identify root causes of XCM failures, and improve observability for future integrations.

## Where to Go Next

<div class="grid cards" markdown>

- <span class="badge external">External</span> **Chopsticks Repository**

    ---

    View the official Chopsticks GitHub repository.

    [:octicons-arrow-right-24: Get Started](https://github.com/AcalaNetwork/chopsticks/)

- <span class="badge guide">Guide</span> **Get Started with XCM**

    ---

    Learn how to use XCM effectively for cross-chain communication.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/transfer-assets-parachains/)

</div>


---

Page Title: Set Up the Polkadot SDK Parachain Template

- Resolved Markdown: https://docs.polkadot.com/parachains/launch-a-parachain/set-up-the-parachain-template.md
- Canonical (HTML): https://docs.polkadot.com/parachains/launch-a-parachain/set-up-the-parachain-template/
- Summary: Learn how to set up and run the Polkadot SDK Parachain Template locally, creating a ready-to-customize foundation for your parachain.
- Word Count: 1627; Token Estimate: 2599
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:8fd837b2a15b663758011b2faafe8f1aa94dc6d514a1783637a10b28c15014be

# Set Up the Polkadot SDK Parachain Template

## Introduction

The [Polkadot SDK](https://github.com/paritytech/polkadot-sdk) includes several [templates](/polkadot-mkdocs/parachains/customize-runtime/#starting-templates) designed to help you quickly start building your own blockchain. Each template offers a different level of configuration, from minimal setups to feature-rich environments, allowing you to choose the foundation that best fits your project's needs.

Among these, the [Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template) provides a preconfigured runtime with commonly used pallets, making it an ideal starting point for most parachain development projects.

This guide walks you through the full process of working with this template. You will:

- Set up the Polkadot SDK Parachain Template.
- Understand the project structure and key components.
- Verify your template is ready for development.
- Run the parachain template locally in development mode.

By the end of this guide, you'll have a working template ready to customize and deploy as a parachain.

## Prerequisites

Before getting started, ensure you have done the following:

- Completed the [Install Polkadot SDK](/polkadot-mkdocs/parachains/install-polkadot-sdk/) guide and successfully installed [Rust](https://rust-lang.org/) and the required packages to set up your development environment.

For this tutorial series, you need to use Rust `1.88`. Newer versions of the compiler may not work with this parachain template version.

Run the following commands to set up the correct Rust version:

=== "macOS"

    ```bash
    rustup install 1.88
    rustup default 1.88
    rustup target add wasm32-unknown-unknown --toolchain 1.88-aarch64-apple-darwin
    rustup component add rust-src --toolchain 1.88-aarch64-apple-darwin
    ```

=== "Ubuntu"

    ```bash
    rustup toolchain install 1.88.0
    rustup default 1.88.0
    rustup target add wasm32-unknown-unknown --toolchain 1.88.0
    rustup component add rust-src --toolchain 1.88.0
    ```

## Polkadot SDK Utility Tools

This tutorial requires two essential tools:

- [**Chain spec builder**](https://crates.io/crates/staging-chain-spec-builder/16.0.0) - a Polkadot SDK utility for generating chain specifications. Refer to the [Generate Chain Specs](/polkadot-mkdocs/parachains/launch-a-parachain/deploy-to-polkadot/#generate-the-chain-specification) documentation for detailed usage
- [**Polkadot Omni Node**](https://crates.io/crates/polkadot-omni-node/0.13.2) - a white-labeled binary, released as a part of Polkadot SDK, that can act as the collator of a parachain in production, with all the related auxiliary functionalities that a normal collator node has: RPC server, archiving state, etc. It can also run the Wasm blob of the parachain locally for testing and development

Download the pre-built binaries from the [Polkadot SDK release](https://github.com/paritytech/polkadot-sdk/releases/tag/polkadot-stable2512-2) (recommended):

=== "macOS"

    ```bash
    curl -L -o chain-spec-builder https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/chain-spec-builder-aarch64-apple-darwin
    curl -L -o polkadot-omni-node https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/polkadot-omni-node-aarch64-apple-darwin
    chmod +x chain-spec-builder polkadot-omni-node
    sudo mv chain-spec-builder polkadot-omni-node /usr/local/bin/
    ```

=== "Ubuntu"

    ```bash
    curl -L -o chain-spec-builder https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/chain-spec-builder
    curl -L -o polkadot-omni-node https://github.com/paritytech/polkadot-sdk/releases/download/polkadot-stable2512-2/polkadot-omni-node
    chmod +x chain-spec-builder polkadot-omni-node
    sudo mv chain-spec-builder polkadot-omni-node /usr/local/bin/
    ```

Alternatively, you can install from source using `cargo`:

```bash
cargo install --locked staging-chain-spec-builder@16.0.0
cargo install --locked polkadot-omni-node@0.13.2
```

## Clone the Template

The [Polkadot SDK Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template) provides a ready-to-use development environment for building with the [Polkadot SDK](https://github.com/paritytech/polkadot-sdk). Follow these steps to set up the template:

1. Clone the template repository:

    ```bash
    git clone https://github.com/paritytech/polkadot-sdk-parachain-template.git parachain-template
    ```

2. Navigate into the project directory:

    ```bash
    cd parachain-template
    ```

## Explore the Project Structure

Before building the template, take a moment to familiarize yourself with its structure. Understanding this organization will help you navigate the codebase as you develop your parachain.

The template follows a standard Polkadot SDK project layout:

```text
parachain-template/
├── node/              # Node implementation and client
├── pallets/           # Custom pallets for your parachain
├── runtime/           # Runtime configuration and logic
├── Cargo.toml         # Workspace configuration
└── README.md          # Documentation
```

Key directories explained:

- **runtime/**: Contains your parachain's state transition function and pallet configuration. This is where you'll define what your blockchain can do.
- **node/**: Houses the client implementation that runs your blockchain, handles networking, and manages the database.
- **pallets/**: Where you'll create custom business logic modules (pallets) for your specific use case.
- **Cargo.toml**: The workspace configuration that ties all components together.

!!!note
    The runtime is compiled to WebAssembly (Wasm), enabling forkless upgrades. The node binary remains constant while the runtime can be updated on-chain.

## Compile the Runtime

Now that you understand the template structure, let's compile the runtime to ensure everything is working correctly.

1. Compile the runtime:

    ```bash
    cargo build --release --locked
    ```

    !!!tip
        Initial compilation may take several minutes, depending on your machine specifications. Use the `--release` flag for improved runtime performance compared to the default `--debug` build. If you need to troubleshoot issues, the `--debug` build provides better diagnostics.
        
        For production deployments, consider using a dedicated `--profile production` flag - this can provide an additional 15-30% performance improvement over the standard `--release` profile.

2. Upon successful compilation, you should see output indicating the build was successful. The compiled runtime will be located at:
    
    `./target/release/wbuild/parachain-template-runtime/parachain_template_runtime.compact.compressed.wasm`

## Verify the Build

After compilation completes, verify that the runtime was created successfully by checking for the Wasm blob:

```bash
ls -la ./target/release/wbuild/parachain-template-runtime/
```

You should see the `parachain_template_runtime.compact.compressed.wasm` file in the output, confirming the build was successful.

## Run the Node Locally

After successfully compiling your runtime, you can spin up a local chain and produce blocks. This process will start your local parachain using the Polkadot Omni Node and allow you to interact with it. You'll first need to generate a chain specification that defines your network's identity, initial connections, and genesis state, providing the foundational configuration for how your nodes connect and what initial state they agree upon.

Follow these steps to launch your node in development mode:

1. Generate the chain specification file of your parachain:

    ```bash
    chain-spec-builder create -t development \
    --relay-chain paseo \
    --para-id 1000 \
    --runtime ./target/release/wbuild/parachain-template-runtime/parachain_template_runtime.compact.compressed.wasm \
    named-preset development
    ```

2. Start the Omni Node with the generated chain spec. You'll start it in development mode (without a relay chain config), producing and finalizing blocks:

    ```bash
    polkadot-omni-node --chain ./chain_spec.json --dev
    ```

    The `--dev` option does the following:

    - Deletes all active data (keys, blockchain database, networking information) when stopped.
    - Ensures a clean working state each time you restart the node.

3. Verify that your node is running by reviewing the terminal output. You should see log messages indicating block production and finalization.

4. Confirm that your blockchain is producing new blocks by checking if the number after `finalized` is increasing in the output.

The details of the log output will be explored in a later tutorial. For now, knowing that your node is running and producing blocks is sufficient.

## Interact with the Node

When running the template node, it's accessible by default at `ws://localhost:9944`. To interact with your node using the [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) interface, follow these steps:

1. Open [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) in your web browser and click the network icon (which should be the Polkadot logo) in the top left corner:
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-01.webp)

2. Connect to your local node:

    1. Scroll to the bottom and select **Development**.
    2. Choose **Custom**.
    3. Enter `ws://localhost:9944` in the **custom endpoint** input field.
    4. Click the **Switch** button.
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-02.webp)

3. Once connected, you should see **parachain-template-runtime** in the top left corner, with the interface displaying information about your local blockchain.
    
    ![](/polkadot-mkdocs/images/parachains/launch-a-parachain/set-up-the-parachain-template/parachain-template-03.webp)

You are now connected to your local node and can interact with it through the Polkadot.js Apps interface. This tool enables you to explore blocks, execute transactions, and interact with your blockchain's features. For in-depth guidance on using the interface effectively, refer to the [Polkadot.js Guides](https://wiki.polkadot.com/general/polkadotjs/) available on the Polkadot Wiki.

## Stop the Node

When you're done exploring your local node, you can stop it to remove any state changes you've made. Since you started the node with the `--dev` option, stopping the node will purge all persistent block data, allowing you to start fresh the next time.

To stop the local node:

1. Return to the terminal window where the node output is displayed.
2. Press `Control-C` to stop the running process.
3. Verify that your terminal returns to the prompt in the `parachain-template` directory.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge tutorial">Tutorial</span> __Deploy to Polkadot__

    ---

    Learn how to deploy your parachain template to a relay chain testnet. Configure your chain specification, register as a parachain, and start producing blocks.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/launch-a-parachain/deploy-to-polkadot/)

</div>


---

Page Title: Smart Contracts Cookbook

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/cookbook.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/cookbook/
- Summary: Explore our full collection of tutorials and guides to learn step-by-step how to build, deploy, and work with smart contracts on Polkadot.
- Word Count: 211; Token Estimate: 1057
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:aa2b9fa4bafe1526738001cad0eb9d1eadb9d2e18238371b5ca938c4f0d080e7

# Smart Contracts Cookbook

Welcome to the Polkadot smart contracts cookbook index.

This page contains a list of all relevant tutorials and guides to help you get started coding smart contracts and dApps in Polkadot.



## Get Tokens from the Faucet

| Title                              | Difficulty  | Tools | Description                                                                                                           |
|------------------------------------|:-----------:|-------|-----------------------------------------------------------------------------------------------------------------------|
| [Faucet](/polkadot-mkdocs/smart-contracts/faucet/) | 🟢 Beginner | N/A   | Learn how to obtain test tokens from Polkadot faucets for development and testing purposes across different networks. |

## EVM Smart Contracts

| Title                                                                                                   | Difficulty  | Tools                          | Description                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------|:-----------:|--------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| [Deploy an ERC-20 to Polkadot Hub](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-erc20/erc20-remix/) | 🟢 Beginner | EVM Wallet, Polkadot Remix IDE | Deploy an ERC-20 token on Polkadot Hub. This guide covers contract creation, compilation, deployment, and interaction via Polkadot Remix IDE. |
| [Deploy an NFT to Polkadot Hub](/polkadot-mkdocs/smart-contracts/cookbook/smart-contracts/deploy-nft/nft-remix/)        | 🟢 Beginner | EVM Wallet, Polkadot Remix IDE | Deploy an NFT on Polkadot Hub with OpenZeppelin. Learn how to compile, deploy, and interact with your contract using Polkadot Remix IDE.      |

## Port Ethereum DApps

| Title                                                                               |   Difficulty    | Tools   | Description                                                                                                                      |
|-------------------------------------------------------------------------------------|:---------------:|---------|----------------------------------------------------------------------------------------------------------------------------------|
| [Deploying Uniswap V2 on Polkadot](/polkadot-mkdocs/smart-contracts/cookbook/eth-dapps/uniswap-v2/) | 🟡 Intermediate | Hardhat | Learn how to deploy and test Uniswap V2 on Polkadot Hub using Hardhat, bringing AMM-based token swaps to the Polkadot ecosystem. |


---

Page Title: Smart Contracts Overview

- Resolved Markdown: https://docs.polkadot.com/smart-contracts/overview.md
- Canonical (HTML): https://docs.polkadot.com/smart-contracts/overview/
- Summary: Learn about smart contract development on Polkadot Hub with native PVM support, dual-VM execution, and seamless cross-chain capabilities.
- Word Count: 584; Token Estimate: 907
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:93ee7445d345cfddc4b9a04522b20a0cf7c9534a1bbafe69bb34913a2f81d22c

# Smart Contracts on Polkadot Hub

## Introduction

Polkadot Hub provides a production-ready smart contract platform that combines Ethereum compatibility with the performance and cross-chain capabilities of the Polkadot ecosystem. Developers can deploy smart contracts directly on Polkadot Hub while using familiar Ethereum tooling, workflows, and programming languages.

Built with a dual-VM approach, Polkadot Hub offers two execution backends: REVM for unmodified EVM compatibility and native PVM for optimized computationally expensive workloads. This dual-VM architecture enables developers to migrate existing Ethereum contracts instantly or optimize for speed and efficiency with native execution.

## Why Build on Polkadot Hub

### Ethereum Compatibility

Deploy existing Ethereum contracts with zero modifications while maintaining full compatibility with your existing development stack:

- **Complete JSON-RPC API support**: Use MetaMask, Hardhat, Remix, Foundry, and all standard Ethereum tooling.
- **Standard libraries**: Integrate Ethers.js, Web3.js, Viem, Wagmi, and Web3.py without changes.
- **Solidity development**: Write contracts in Solidity or migrate existing code directly. Use the [OpenZeppelin Contracts Wizard for Polkadot](https://wizard.openzeppelin.com/polkadot) to generate secure ERC-20, ERC-721, and other OpenZeppelin-standard contracts.
- **Familiar workflows**: Maintain your existing deployment, testing, and monitoring processes.

### Performance Options

Choose between two execution backends:

- **REVM**: Run unmodified Ethereum contracts with full EVM/Ethereum compatibility.
- **PVM**: Compile to optimized RISC-V bytecode for enhanced performance and lower fees while keeping Ethereum-compatibility. You can write PVM contracts in Solidity (via `resolc`) or Rust—for Rust, tooling is maturing; use LLMs and coding agents to assist development.

Both backends share the same RPC interface and tooling support, allowing seamless transitions. In addition, smart contracts can interact with Polkadot native services via [precompile contracts](/polkadot-mkdocs/smart-contracts/precompiles/).

### Cross-VM  & Cross-Chain Capabilities

Smart contracts written for one VM (for example, EVM) can interact directly with other smart contracts written for the RISC-V PVM, and back. This allows to use full EVM compatible contracts but extend to heavy/complex execution workloads to the PVM RISC-V backend.

Furthermore, all smart contracts in Polkadot Hub can interact with any service in the Polkadot ecosystem through [XCM](/polkadot-mkdocs/smart-contracts/precompiles/xcm/), enabling token transfers, remote execution, and cross-chain composability without bridges or intermediaries.

## Other Smart Contract Environments

Beyond Polkadot Hub's native EVM and PVM support, the ecosystem offers one main alternative for smart contract development:

- **EVM-compatible parachains**: Provide access to Ethereum's extensive developer ecosystem, smart contract portability, and established tooling like Hardhat, Remix, Foundry, and OpenZeppelin. The main options include Moonbeam (the first full Ethereum-compatible parachain serving as an interoperability hub), Astar (featuring dual VM support for both EVM and WebAssembly contracts), and Acala (DeFi-focused with enhanced Acala EVM+ offering advanced DeFi primitives).

## Next Steps

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Get Started__

    ---

    Quick-start guides for connecting, deploying, and building your first smart contract.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/smart-contracts/get-started/)

-   <span class="badge guide">Guide</span> __Cookbook__

    ---

    Step-by-step tutorials for deploying contracts, tokens, NFTs, and full dApps.

    [:octicons-arrow-right-24: View Tutorials](/polkadot-mkdocs/smart-contracts/cookbook/)

-   <span class="badge guide">Guide</span> __Ethereum Developers__

    ---

    Understand key differences in accounts, fees, gas model, and deployment on Polkadot Hub.

    [:octicons-arrow-right-24: Learn More](/polkadot-mkdocs/smart-contracts/for-eth-devs/accounts/)

-   <span class="badge guide">Guide</span> __Precompiles__

    ---

    Discover advanced functionalities including XCM for cross-chain interactions.

    [:octicons-arrow-right-24: Explore Precompiles](/polkadot-mkdocs/smart-contracts/precompiles/)

</div>


---

Page Title: Support

- Resolved Markdown: https://docs.polkadot.com/get-support.md
- Canonical (HTML): https://docs.polkadot.com/get-support/
- Summary: Start here to get developer support for Polkadot. Connect with the team, find help, and explore resources beyond the documentation.
- Word Count: 584; Token Estimate: 1322
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:6dc87403402519eb3a92b6e63d6981e26b1ceebfbf20534d2054c1da475d14cc

# Polkadot Developer Support

You're already in the docs — solid start. But sometimes you need more: answers, real examples, someone to talk to. This support hub is here to help you move forward — faster.

If something’s missing, unclear, or broken — **[tell us](https://github.com/polkadot-developers/polkadot-docs/issues/new?template=docs-issue.yml)**. Your feedback makes the whole ecosystem better for everyone.

## Support Channels

Use one of the channels below to get live technical support or ask questions.

<div class="grid cards support" markdown>

-   :simple-telegram:{ .sub } **Telegram: Polkadot Developer Support**

    <ul class="card-list">
    <li> **Who’s there:** DevRel team and active developer community. </li>
    <li> **Response time:** Within **2 business days (usually faster)**. </li>
    <li> **Topics:** Any developer-related question is welcome. </li>
    </ul>

    👉 [Join Telegram](https://t.me/substratedevs)

-   :simple-discord:{ .sub } **Discord: Polkadot Official Server**
 
    <ul class="card-list">
    <li> **Smart contracts:** Ask in `#solidity-smart-contracts` and `#ink_smart-contracts`. </li>
    <li> **General developer support:** Ask in `#solidity-smart-contracts`. </li>
    <li> **Response time:** Within **1 business day (usually faster)**. </li>
    </ul>

    👉 [Join Discord](https://polkadot-discord.w3f.tools/)

-   :simple-matrix:{ .sub } **Matrix: Polkadot Developer Support**

    <ul class="card-list">
    <li> **Who’s there:** Parity, W3F, DevRel, and community contributors. </li>
    <li> **Response time:** Within **1 business day (usually faster)**. </li>
    <li> **Topics:** Full-spectrum developer support. </li>
    <li> Bridged with Telegram (all messages synced). </li>
    </ul>

    👉 [Join Matrix](https://matrix.to/#/#substratedevs:matrix.org)

</div>

## Community Resources

<div class="grid cards support" markdown>

-   :fontawesome-brands-stack-exchange:{ .sub } **Stack Exchange**
    
    <ul class="card-list">
    <li> Browse commonly asked technical questions. </li>
    <li> Ask your own and get detailed responses from experienced devs. </li>
    </ul>

    👉 [Visit Polkadot Stack Exchange](https://substrate.stackexchange.com/)

-   :simple-reddit:{ .sub } **Reddit: r/Polkadot**
    
    <ul class="card-list">
    <li> General discussions and community perspectives. </li>
    <li> Developer questions are welcome — just tag them appropriately. </li>
    </ul>

    👉 [Visit r/Polkadot](https://www.reddit.com/r/Polkadot/)

-   :simple-youtube:{ .sub } **YouTube: @PolkadotNetwork**
    
    <ul class="card-list">
    <li> Developer tutorials. </li>
    <li> Ecosystem interviews. </li>
    <li> Event recordings and walkthroughs. </li>
    </ul>

    👉 [Watch on YouTube](https://www.youtube.com/@PolkadotNetwork)

-   :fontawesome-brands-x-twitter:{ .sub } **X (Twitter): Official Accounts**
    
    - **[@PolkadotDevs](https://x.com/PolkadotDevs)**: Updates for developers.
    - **[@Polkadot](https://x.com/Polkadot)**: Network-wide news.
    - **[@Kusamanetwork](https://x.com/kusamanetwork)**: Kusama-specific updates.
    - **[@Web3Foundation](https://x.com/web3foundation)**: Grants, research, and ecosystem programs.

-   :fontawesome-brands-x-twitter:{ .sub } **X (Twitter): Community Accounts**

    - **[@PolkadotDeploy](https://x.com/PolkadotDeploy)**: News from the deployment portal and tooling updates.

-   :material-forum:{ .sub } **Polkadot Forum**
    
    <ul class="card-list">
    <li> Join community discussions around the direction of the ecosystem. </li>
    </ul>

    👉 [Visit the Polkadot Forum](https://forum.polkadot.network/)

-   :material-vote:{ .sub } **Polkassembly: OpenGov**
    
    <ul class="card-list">
    <li> Explore and vote on governance proposals for Polkadot and Kusama. </li>
    <li> Help shape the future of the network. </li>
    </ul>

    👉 [Explore on Polkassembly](https://polkadot.polkassembly.io/)

-   :material-vote:{ .sub } **Subsquare: OpenGov**

    <ul class="card-list">
    <li> Track, discuss, and vote on Polkadot OpenGov proposals. </li>
    <li> Stay informed on governance activity across the network. </li>
    </ul>

    👉 [Explore on Subsquare](https://polkadot.subsquare.io/)

</div>

## AI Resources

<div class="grid cards support" markdown>

-   :fontawesome-solid-robot:{ .sub } **AI Resources**

    Access documentation structured and optimized for use with large language models (LLMs) and AI tools. These resources help build AI assistants, power code search, or enable custom tooling trained on Polkadot’s documentation.

    👉 [Access LLM Files](/polkadot-mkdocs/ai-resources/)

</div>


---

Page Title: Technical Reference Overview

- Resolved Markdown: https://docs.polkadot.com/reference.md
- Canonical (HTML): https://docs.polkadot.com/reference/
- Summary: Learn about Polkadot's technical architecture, governance framework, parachain ecosystem, and the tools you need to build and interact with the network.
- Word Count: 946; Token Estimate: 1659
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:605f2db2ea0b66982074dac53299fb4d9c53ff612a32e01d16378516ef97db43

## Introduction

The Technical Reference section provides comprehensive documentation of Polkadot's architecture, core concepts, and development tooling. Whether you're exploring how Polkadot's relay chain coordinates parachains, understanding governance mechanisms, or building applications on the network, this reference covers the technical foundations you need.

Polkadot is a multi-chain network that enables diverse, interconnected blockchains to share security and communicate seamlessly. Understanding how these components interact from the [relay chain](/polkadot-mkdocs/reference/glossary#relay-chain) that validates [parachains](/polkadot-mkdocs/reference/glossary#parachain) to the [governance](/polkadot-mkdocs/reference/glossary#governance) mechanisms that evolve the protocol is essential for developers, validators, and network participants.

This guide organizes technical documentation across five core areas: Polkadot Hub, Parachains, On-Chain Governance, Glossary, and Tools, each providing detailed information on different aspects of the Polkadot ecosystem.

## Polkadot Hub

[Polkadot Hub](/polkadot-mkdocs/reference/polkadot-hub/) is the entry point to Polkadot for all users and application developers. It provides access to essential Web3 services including smart contracts, asset management, staking, governance, identity management, and cross-ecosystem interoperability—without requiring you to deploy or manage a parachain.

The Hub encompasses a set of core functionality that enables developers and users to build and interact with applications on Polkadot. Key capabilities include:

- **Smart contracts**: Deploy Ethereum-compatible smart contracts and build decentralized applications.
- **Asset management**: Create, manage, and transfer fungible tokens and NFTs across the ecosystem.
- **Staking**: Participate in network security and earn rewards by staking DOT.
- **Governance**: Vote on proposals and participate in Polkadot's decentralized decision-making through OpenGov.
- **Identity services**: Register and manage on-chain identities, enabling access to governance roles and network opportunities.
- **Cross-chain interoperability**: Leverage XCM messaging to interact securely with other chains in the Polkadot ecosystem.
- **Collectives and DAOs**: Participate in governance collectives and decentralized autonomous organizations.

## Parachains

[Parachains](/polkadot-mkdocs/reference/parachains/) are specialized blockchains that connect to the Polkadot relay chain, inheriting its security while maintaining their own application-specific logic. The parachains documentation covers:

- **Accounts**: Deep dive into account types, storage, and management on parachains.
- **Blocks, transactions and fees**: Understand block production, transaction inclusion, and fee mechanisms.
- **Consensus**: Learn how parachain blocks are validated and finalized through the relay chain's consensus.
- **Chain data**: Explore data structures, storage layouts, and state management.
- **Cryptography**: Study cryptographic primitives used in Polkadot SDK-based chains.
- **Data encoding**: Understand how data is encoded and decoded for blockchain compatibility.
- **Networks**: Learn about networking protocols and peer-to-peer communication.
- **Interoperability**: Discover [Cross-Consensus Messaging (XCM)](/polkadot-mkdocs/parachains/interoperability/get-started/), the standard for cross-chain communication.
- **Randomness**: Understand how randomness is generated and used in Polkadot chains.
- **Node and runtime**: Learn about parachain nodes, runtime environments, and the [Polkadot SDK](https://github.com/paritytech/polkadot-sdk).

## On-Chain Governance

[On-Chain governance](/polkadot-mkdocs/reference/governance/) is the decentralized decision-making mechanism for the Polkadot network. It manages the evolution and modification of the network's runtime logic, enabling community oversight and approval for proposed changes. The governance documentation details:

- **OpenGov framework**: Understand Polkadot's next-generation governance system with enhanced delegation, flexible tracks, and simultaneous referendums.
- **Origins and tracks**: Learn how governance proposals are categorized, prioritized, and executed based on their privilege level and complexity.
- **Voting and delegation**: Explore conviction voting, vote delegation, and how token holders participate in governance.
- **Governance evolution**: See how Polkadot's governance has evolved from Governance V1 to the current OpenGov system.

## Glossary

The [Glossary](/polkadot-mkdocs/reference/glossary/) provides quick-reference definitions for Polkadot-specific terminology. Essential terms include:

- Blockchain concepts (blocks, transactions, state)
- Consensus mechanisms (validators, collators, finality)
- Polkadot-specific terms (relay chain, parachain, XCM, FRAME)
- Network components (nodes, runtimes, storage)
- Governance terminology (origins, tracks, referendums)

## Tools

The [Tools](/polkadot-mkdocs/reference/tools/zombienet/) section documents essential development and interaction tools for the Polkadot ecosystem:

- **Light clients**: Lightweight solutions for interacting with the network without running full nodes.
- **JavaScript/TypeScript tools**: Libraries like [Polkadot.js API](/polkadot-mkdocs/reference/tools/polkadot-js-api/) and [PAPI](/polkadot-mkdocs/reference/tools/papi/) for building applications.
- **Rust tools**: [Polkadart](/polkadot-mkdocs/reference/tools/polkadart/) and other Rust-based libraries for SDK development.
- **Python tools**: [py-substrate-interface](/polkadot-mkdocs/reference/tools/py-substrate-interface/) for Python developers.
- **Testing and development**: Tools like [Moonwall](/polkadot-mkdocs/reference/tools/moonwall/), [Chopsticks](/polkadot-mkdocs/reference/tools/chopsticks/), and [Omninode](/polkadot-mkdocs/reference/tools/omninode/) for smart contract and parachain testing.
- **Indexing and monitoring**: [Sidecar](/polkadot-mkdocs/reference/tools/sidecar/) for data indexing and [Dedot](/polkadot-mkdocs/reference/tools/dedot/) for substrate interaction.
- **Cross-chain tools**: [ParaSpell](/polkadot-mkdocs/reference/tools/paraspell/) for XCM integration and asset transfers.

## Where to Go Next

For detailed exploration of specific areas, proceed to any of the main sections:

<div class="grid cards" markdown>

- <span class="badge learn">Learn</span> **Polkadot Hub**

    ---

    Understand the relay chain's role in coordinating parachains, providing shared security, and enabling governance.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/polkadot-hub/)

- <span class="badge learn">Learn</span> **Parachains**

    ---

    Deep dive into parachain architecture, consensus, data structures, and building application-specific blockchains.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/parachains/)

- <span class="badge learn">Learn</span> **On-Chain Governance**

    ---

    Explore Polkadot's decentralized governance framework and how to participate in network decision-making.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/governance/)

- <span class="badge guide">Guide</span> **Glossary**

    ---

    Quick reference for Polkadot-specific terminology and concepts used throughout the documentation.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/glossary/)

- <span class="badge guide">Guide</span> **Tools**

    ---

    Discover development tools, libraries, and frameworks for building and interacting with Polkadot.

    [:octicons-arrow-right-24: Reference](/polkadot-mkdocs/reference/tools/zombienet/)

</div>


---

Page Title: Transactions

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/transactions.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/transactions/
- Summary: Learn how to construct, submit, and validate transactions in the Polkadot SDK, covering signed, unsigned, and inherent types of transactions.
- Word Count: 3302; Token Estimate: 4615
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:9099c1f8fcc1506f86c7015248ee9001f62b3bc1f03e60ce49571d50e33cf09a

# Transactions

## Introduction

Transactions are essential components of blockchain networks, enabling state changes and the execution of key operations. In the Polkadot SDK, transactions, often called extrinsics, come in multiple forms, including signed, unsigned, and inherent transactions.

This guide walks you through the different transaction types and how they're formatted, validated, and processed within the Polkadot ecosystem. You'll also learn how to customize transaction formats and construct transactions for FRAME-based runtimes, ensuring a complete understanding of how transactions are built and executed in Polkadot SDK-based chains.

## What Is a Transaction?

In the Polkadot SDK, transactions represent operations that modify the chain's state, bundled into blocks for execution. The term extrinsic is often used to refer to any data that originates outside the runtime and is included in the chain. While other blockchain systems typically refer to these operations as "transactions," the Polkadot SDK adopts the broader term "extrinsic" to capture the wide variety of data types that can be added to a block.

There are three primary types of transactions (extrinsics) in the Polkadot SDK:

- **Signed transactions**: Signed by the submitting account, often carrying transaction fees.
- **Unsigned transactions**: Submitted without a signature, often requiring custom validation logic.
- **Inherent transactions**: Typically inserted directly into blocks by block authoring nodes, without gossiping between peers.

Each type serves a distinct purpose, and understanding when and how to use each is key to efficiently working with the Polkadot SDK.

### Signed Transactions

Signed transactions require an account's signature and typically involve submitting a request to execute a runtime call. The signature serves as a form of cryptographic proof that the sender has authorized the action, using their private key. These transactions often involve a transaction fee to cover the cost of execution and incentivize block producers.

Signed transactions are the most common type of transaction and are integral to user-driven actions, such as token transfers. For instance, when you transfer tokens from one account to another, the sending account must sign the transaction to authorize the operation.

For example, the [`pallet_balances::Call::transfer_allow_death`](https://paritytech.github.io/polkadot-sdk/master/pallet_balances/pallet/struct.Pallet.html#method.transfer_allow_death) extrinsic in the Balances pallet allows you to transfer tokens. Since your account initiates this transaction, your account key is used to sign it. You'll also be responsible for paying the associated transaction fee, with the option to include an additional tip to incentivize faster inclusion in the block.

### Unsigned Transactions

Unsigned transactions do not require a signature or account-specific data from the sender. Unlike signed transactions, they do not come with any form of economic deterrent, such as fees, which makes them susceptible to spam or replay attacks. Custom validation logic must be implemented to mitigate these risks and ensure these transactions are secure.

Unsigned transactions typically involve scenarios where including a fee or signature is unnecessary or counterproductive. However, due to the absence of fees, they require careful validation to protect the network. For example, [`pallet_im_online::Call::heartbeat`](https://paritytech.github.io/polkadot-sdk/master/pallet_im_online/pallet/struct.Pallet.html#method.heartbeat) extrinsic allows validators to send a heartbeat signal, indicating they are active. Since only validators can make this call, the logic embedded in the transaction ensures that the sender is a validator, making the need for a signature or fee redundant.

Unsigned transactions are more resource-intensive than signed ones because custom validation is required, but they play a crucial role in certain operational scenarios, especially when regular user accounts aren't involved.

### Inherent Transactions

Inherent transactions are a specialized type of unsigned transaction that is used primarily for block authoring. Unlike signed or other unsigned transactions, inherent transactions are added directly by block producers and are not broadcasted to the network or stored in the transaction queue. They don't require signatures or the usual validation steps and are generally used to insert system-critical data directly into blocks.

A key example of an inherent transaction is inserting a timestamp into each block. The [`pallet_timestamp::Call::now`](https://paritytech.github.io/polkadot-sdk/master/pallet_timestamp/pallet/struct.Pallet.html#method.now-1) extrinsic allows block authors to include the current time in the block they are producing. Since the block producer adds this information, there is no need for transaction validation, like signature verification. The validation in this case is done indirectly by the validators, who check whether the timestamp is within an acceptable range before finalizing the block.

Another example is the [`paras_inherent::Call::enter`](https://paritytech.github.io/polkadot-sdk/master/polkadot_runtime_parachains/paras_inherent/pallet/struct.Pallet.html#method.enter) extrinsic, which enables parachain collator nodes to send validation data to the relay chain. This inherent transaction ensures that the necessary parachain data is included in each block without the overhead of gossiped transactions.

Inherent transactions serve a critical role in block authoring by allowing important operational data to be added directly to the chain without needing the validation processes required for standard transactions.

## Transaction Formats

Understanding the structure of signed and unsigned transactions is crucial for developers building on Polkadot SDK-based chains. Whether you're optimizing transaction processing, customizing formats, or interacting with the transaction pool, knowing the format of extrinsics, Polkadot's term for transactions, is essential.

### Types of Transaction Formats

In Polkadot SDK-based chains, extrinsics can fall into three main categories:

- **Unchecked extrinsics**: Typically used for signed transactions that require validation. They contain a signature and additional data, such as a nonce and information for fee calculation. Unchecked extrinsics are named as such because they require validation checks before being accepted into the transaction pool.
- **Checked extrinsics**: Typically used for inherent extrinsics (unsigned transactions); these don't require signature verification. Instead, they carry information such as where the extrinsic originates and any additional data required for the block authoring process.
- **Opaque extrinsics**: Used when the format of an extrinsic is not yet fully committed or finalized. They are still decodable, but their structure can be flexible depending on the context.

### Signed Transaction Data Structure

A signed transaction typically includes the following components:

- **Signature**: Verifies the authenticity of the transaction sender.
- **Call**: The actual function or method call the transaction is requesting (for example, transferring funds).
- **Nonce**: Tracks the number of prior transactions sent from the account, helping to prevent replay attacks.
- **Tip**: An optional incentive to prioritize the transaction in block inclusion.
- **Additional data**: Includes details such as spec version, block hash, and genesis hash to ensure the transaction is valid within the correct runtime and chain context.

Here's a simplified breakdown of how signed transactions are typically constructed in a Polkadot SDK runtime:

``` code
<signing account ID> + <signature> + <additional data>
```

Each part of the signed transaction has a purpose, ensuring the transaction's authenticity and context within the blockchain.

### Signed Extensions

Polkadot SDK also provides the concept of [signed extensions](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/signed_extensions/index.html), which allow developers to extend extrinsics with additional data or validation logic before they are included in a block. The [`SignedExtension`](https://paritytech.github.io/try-runtime-cli/sp_runtime/traits/trait.SignedExtension.html) set helps enforce custom rules or protections, such as ensuring the transaction's validity or calculating priority.

The transaction queue regularly calls signed extensions to verify a transaction's validity before placing it in the ready queue. This safeguard ensures transactions won't fail in a block. Signed extensions are commonly used to enforce validation logic and protect the transaction pool from spam and replay attacks.

In FRAME, a signed extension can hold any of the following types by default:

- **[`AccountId`](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_frame/runtime/types_common/type.AccountId.html)**: To encode the sender's identity.
- **[`Call`](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_frame/traits/trait.SignedExtension.html#associatedtype.Call)**: To encode the pallet call to be dispatched. This data is used to calculate transaction fees.
- **[`AdditionalSigned`](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_frame/traits/trait.SignedExtension.html#associatedtype.AdditionalSigned)**: To handle any additional data to go into the signed payload allowing you to attach any custom logic prior to dispatching a transaction.
- **[`Pre`](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_frame/traits/trait.SignedExtension.html#associatedtype.Pre)**: To encode the information that can be passed from before a call is dispatched to after it gets dispatched.

Signed extensions can enforce checks like:

- **[`CheckSpecVersion`](https://paritytech.github.io/polkadot-sdk/master/src/frame_system/extensions/check_spec_version.rs.html)**: Ensures the transaction is compatible with the runtime's current version.
- **[`CheckWeight`](https://paritytech.github.io/polkadot-sdk/master/frame_system/struct.CheckWeight.html)**: Calculates the weight (or computational cost) of the transaction, ensuring the block doesn't exceed the maximum allowed weight.

These extensions are critical in the transaction lifecycle, ensuring that only valid and prioritized transactions are processed.

## Transaction Construction

Building transactions in the Polkadot SDK involves constructing a payload that can be verified, signed, and submitted for inclusion in a block. Each runtime in the Polkadot SDK has its own rules for validating and executing transactions, but there are common patterns for constructing a signed transaction.

### Construct a Signed Transaction

A signed transaction in the Polkadot SDK includes various pieces of data to ensure security, prevent replay attacks, and prioritize processing. Here's an overview of how to construct one:

1. **Construct the unsigned payload**: Gather the necessary information for the call, including:

    - **Pallet index**: Identifies the pallet where the runtime function resides.
    - **Function index**: Specifies the particular function to call in the pallet.
    - **Parameters**: Any additional arguments required by the function call.

2. **Create a signing payload**: Once the unsigned payload is ready, additional data must be included:

    - **Transaction nonce**: Unique identifier to prevent replay attacks.
    - **Era information**: Defines how long the transaction is valid before it's dropped from the pool.
    - **Block hash**: Ensures the transaction doesn't execute on the wrong chain or fork.

3. **Sign the payload**: Using the sender's private key, sign the payload to ensure that the transaction can only be executed by the account holder.
4. **Serialize the signed payload**: Once signed, the transaction must be serialized into a binary format, ensuring the data is compact and easy to transmit over the network.
5. **Submit the serialized transaction**: Finally, submit the serialized transaction to the network, where it will enter the transaction pool and wait for processing by an authoring node.

The following is an example of how a signed transaction might look:

``` rust
node_runtime::UncheckedExtrinsic::new_signed(
    function.clone(),                                      // some call
    sp_runtime::AccountId32::from(sender.public()).into(), // some sending account
    node_runtime::Signature::Sr25519(signature.clone()),   // the account's signature
    extra.clone(),                                         // the signed extensions
)
```

### Transaction Encoding

Before a transaction is sent to the network, it is serialized and encoded using a structured encoding process that ensures consistency and prevents tampering:

- **`[1]`**: Compact encoded length in bytes of the entire transaction.
- **`[2]`**: A u8 containing 1 byte to indicate whether the transaction is signed or unsigned (1 bit) and the encoded transaction version ID (7 bits).
- **`[3]`**: If signed, this field contains an account ID, an SR25519 signature, and some extra data.
- **`[4]`**: Encoded call data, including pallet and function indices and any required arguments.

This encoded format ensures consistency and efficiency in processing transactions across the network. By adhering to this format, applications can construct valid transactions and pass them to the network for execution.

To learn more about how compact encoding works using SCALE, see the [SCALE Codec](https://github.com/paritytech/parity-scale-codec) README on GitHub.

### Customize Transaction Construction

Although the basic steps for constructing transactions are consistent across Polkadot SDK-based chains, developers can customize transaction formats and validation rules. For example:

- **Custom pallets**: You can define new pallets with custom function calls, each with its own parameters and validation logic.
- **Signed extensions**: Developers can implement custom extensions that modify how transactions are prioritized, validated, or included in blocks.

By leveraging Polkadot SDK's modular design, developers can create highly specialized transaction logic tailored to their chain's needs.

## Lifecycle of a Transaction

In the Polkadot SDK, transactions are often referred to as extrinsics because the data in transactions originates outside of the runtime. These transactions contain data that initiates changes to the chain state. The most common type of extrinsic is a signed transaction, which is cryptographically verified and typically incurs a fee. This section focuses on how signed transactions are processed, validated, and ultimately included in a block.

### Define Transaction Properties

The Polkadot SDK runtime defines key transaction properties, such as:

- **Transaction validity**: Ensures the transaction meets all runtime requirements.
- **Signed or unsigned**: Identifies whether a transaction needs to be signed by an account.
- **State changes**: Determines how the transaction modifies the state of the chain.

Pallets, which compose the runtime's logic, define the specific transactions that your chain supports. When a user submits a transaction, such as a token transfer, it becomes a signed transaction, verified by the user's account signature. If the account has enough funds to cover fees, the transaction is executed, and the chain's state is updated accordingly.

### Process on a Block Authoring Node

In Polkadot SDK-based networks, some nodes are authorized to author blocks. These nodes validate and process transactions. When a transaction is sent to a node that can produce blocks, it undergoes a lifecycle that involves several stages, including validation and execution. Non-authoring nodes gossip the transaction across the network until an authoring node receives it. The following diagram illustrates the lifecycle of a transaction that's submitted to a network and processed by an authoring node.

![Transaction lifecycle diagram](/polkadot-mkdocs/images/reference/parachains/blocks-transactions-fees/transactions/transactions-01.webp)

### Validate and Queue

Once a transaction reaches an authoring node, it undergoes an initial validation process to ensure it meets specific conditions defined in the runtime. This validation includes checks for:

- **Correct nonce**: Ensures the transaction is sequentially valid for the account.
- **Sufficient funds**: Confirms the account can cover any associated transaction fees.
- **Signature validity**: Verifies that the sender's signature matches the transaction data.

After these checks, valid transactions are placed in the transaction pool, where they are queued for inclusion in a block. The transaction pool regularly re-validates queued transactions to ensure they remain valid before being processed. To reach consensus, two-thirds of the nodes must agree on the order of the transactions executed and the resulting state change. Transactions are validated and queued on the local node in a transaction pool to prepare for consensus.

#### Transaction Pool

The transaction pool is responsible for managing valid transactions. It ensures that only transactions that pass initial validity checks are queued. Transactions that fail validation, expire, or become invalid for other reasons are removed from the pool.

The transaction pool organizes transactions into two queues:

- **Ready queue**: Transactions that are valid and ready to be included in a block.
- **Future queue**: Transactions that are not yet valid but could be in the future, such as transactions with a nonce too high for the current state.

Details on how the transaction pool validates transactions, including fee and signature handling, can be found in the [`validate_transaction`](https://paritytech.github.io/polkadot-sdk/master/sp_transaction_pool/runtime_api/trait.TaggedTransactionQueue.html#method.validate_transaction) method.

#### Invalid Transactions

If a transaction is invalid, for example, due to an invalid signature or insufficient funds, it is rejected and won't be added to the block. Invalid transactions might be rejected for reasons such as:

- The transaction has already been included in a block.
- The transaction's signature does not match the sender.
- The transaction is too large to fit in the current block.

### Transaction Ordering and Priority

When a node is selected as the next block author, it prioritizes transactions based on weight, length, and tip amount. The goal is to fill the block with high-priority transactions without exceeding its maximum size or computational limits. Transactions are ordered as follows:

- **Inherents first**: Inherent transactions, such as block timestamp updates, are always placed first.
- **Nonce-based ordering**: Transactions from the same account are ordered by their nonce.
- **Fee-based ordering**: Among transactions with the same nonce or priority level, those with higher fees are prioritized.

### Transaction Execution

Once a block author selects transactions from the pool, the transactions are executed in priority order. As each transaction is processed, the state changes are written directly to the chain's storage. It's important to note that these changes are not cached, meaning a failed transaction won't revert earlier state changes, which could leave the block in an inconsistent state.

Events are also written to storage. Runtime logic should not emit an event before performing the associated actions. If the associated transaction fails after the event was emitted, the event will not revert.

## Transaction Mortality

Transactions in the network can be configured as either mortal (with expiration) or immortal (without expiration). Every transaction payload contains a block checkpoint (reference block number and hash) and an era/validity period that determines how many blocks after the checkpoint the transaction remains valid.

When a transaction is submitted, the network validates it against these parameters. If the transaction is not included in a block within the specified validity window, it is automatically removed from the transaction queue.

- **Mortal transactions**: Have a finite lifespan and will expire after a specified number of blocks. For example, a transaction with a block checkpoint of 1000 and a validity period of 64 blocks will be valid from blocks 1000 to 1064.

- **Immortal transactions**: Never expire and remain valid indefinitely. To create an immortal transaction, set the block checkpoint to 0 (genesis block), use the genesis hash as a reference, and set the validity period to 0.

However, immortal transactions pose significant security risks through replay attacks. If an account is reaped (balance drops to zero, account removed) and later re-funded, malicious actors can replay old immortal transactions.

The blockchain maintains only a limited number of prior block hashes for reference validation, called `BlockHashCount`. If your validity period exceeds `BlockHashCount`, the effective validity period becomes the minimum of your specified period and the block hash count.

## Unique Identifiers for Extrinsics

Transaction hashes are **not unique identifiers** in Polkadot SDK-based chains.

Key differences from traditional blockchains:

- Transaction hashes serve only as fingerprints of transaction information.
- Multiple valid transactions can share the same hash.
- Hash uniqueness assumptions lead to serious issues.

For example, when an account is reaped (removed due to insufficient balance) and later recreated, it resets to nonce 0, allowing identical transactions to be valid at different points:

| Block | Extrinsic Index | Hash | Origin    | Nonce | Call                | Result                        |
|-------|----------------|------|-----------|-------|---------------------|-------------------------------|
| 100   | 0              | 0x01 | Account A | 0     | Transfer 5 DOT to B | Account A reaped              |
| 150   | 5              | 0x02 | Account B | 4     | Transfer 7 DOT to A | Account A created (nonce = 0) |
| 200   | 2              | 0x01 | Account A | 0     | Transfer 5 DOT to B | Successful transaction        |

Notice that blocks 100 and 200 contain transactions with identical hashes (0x01) but are completely different, valid operations occurring at different times.

Additional complexity comes from Polkadot SDK's origin abstraction. Origins can represent collectives, governance bodies, or other non-account entities that don't maintain nonces like regular accounts and might dispatch identical calls multiple times with the same hash values. Each execution occurs in different chain states with different results.

The correct way to uniquely identify an extrinsic on a Polkadot SDK-based chain is to use the block ID (height or hash) and the extrinsic index. Since the Polkadot SDK defines blocks as headers plus ordered arrays of extrinsics, the index position within a canonical block provides guaranteed uniqueness.

## Additional Resources

For a video overview of the lifecycle of transactions and the types of transactions that exist, see the [Transaction lifecycle](https://www.youtube.com/watch?v=3pfM0GOp02c) seminar from Parity Tech.


---

Page Title: Transactions Weights and Fees

- Resolved Markdown: https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/fees.md
- Canonical (HTML): https://docs.polkadot.com/reference/parachains/blocks-transactions-fees/fees/
- Summary: Overview of transaction weights and fees in Polkadot SDK chains, detailing how fees are calculated using a defined formula and runtime specifics.
- Word Count: 2856; Token Estimate: 4261
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:4451a2e19daa4afb85d4fe3311233868ee6513a567c2cd867912bd5f4590ae61

# Transactions Weights and Fees

## Introductions

When transactions are executed, or data is stored on-chain, the activity changes the chain's state and consumes blockchain resources. Because the resources available to a blockchain are limited, managing how operations on-chain consume them is important. In addition to being limited in practical terms, such as storage capacity, blockchain resources represent a potential attack vector for malicious users. For example, a malicious user might attempt to overload the network with messages to stop the network from producing new blocks. To protect blockchain resources from being drained or overloaded, you need to manage how they are made available and how they are consumed. The resources to be aware of include:

- Memory usage
- Storage input and output
- Computation
- Transaction and block size
- State database size

The Polkadot SDK provides block authors with several ways to manage access to resources and to prevent individual components of the chain from consuming too much of any single resource. Two of the most important mechanisms available to block authors are weights and transaction fees.

[Weights](/polkadot-mkdocs/reference/glossary/#weight) manage the time it takes to validate a block and characterize the time it takes to execute the calls in the block's body. By controlling the execution time a block can consume, weights set limits on storage input, output, and computation.

Some of the weight allowed for a block is consumed as part of the block's initialization and finalization. The weight might also be used to execute mandatory inherent extrinsic calls. To help ensure blocks don’t consume too much execution time and prevent malicious users from overloading the system with unnecessary calls, weights are combined with transaction fees.

[Transaction fees](/polkadot-mkdocs/reference/parachains/blocks-transactions-fees/transactions/#transaction-fees) provide an economic incentive to limit execution time, computation, and the number of calls required to perform operations. Transaction fees are also used to make the blockchain economically sustainable because they are typically applied to transactions initiated by users and deducted before a transaction request is executed.

## How Fees are Calculated

The final fee for a transaction is calculated using the following parameters:

- **`base fee`**: This is the minimum amount a user pays for a transaction. It is declared a base weight in the runtime and converted to a fee using the [`WeightToFee`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.WeightToFee) conversion.
- **`weight fee`**: A fee proportional to the execution time (input and output and computation) that a transaction consumes.
- **`length fee`**: A fee proportional to the encoded length of the transaction.
- **`tip`**: An optional tip to increase the transaction’s priority, giving it a higher chance to be included in the transaction queue.

The base fee and proportional weight and length fees constitute the inclusion fee. The inclusion fee is the minimum fee that must be available for a transaction to be included in a block.

```text
inclusion fee = base fee + weight fee + length fee
```

Transaction fees are withdrawn before the transaction is executed. After the transaction is executed, the weight can be adjusted to reflect the resources used. If a transaction uses fewer resources than expected, the transaction fee is corrected, and the adjusted transaction fee is deposited.

## Using the Transaction Payment Pallet

The [Transaction Payment pallet](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/substrate/frame/transaction-payment) provides the basic logic for calculating the inclusion fee. You can also use the Transaction Payment pallet to:

- Convert a weight value into a deductible fee based on a currency type using [`Config::WeightToFee`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.WeightToFee).
- Update the fee for the next block by defining a multiplier based on the chain’s final state at the end of the previous block using [`Config::FeeMultiplierUpdate`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.FeeMultiplierUpdate).
- Manage the withdrawal, refund, and deposit of transaction fees using [`Config::OnChargeTransaction`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.OnChargeTransaction).

You can learn more about these configuration traits in the [Transaction Payment documentation](https://paritytech.github.io/polkadot-sdk/master/pallet_transaction_payment/index.html).

### Understanding the Inclusion Fee

The formula for calculating the inclusion fee is as follows:

```text
inclusion_fee = base_fee + length_fee + [targeted_fee_adjustment * weight_fee]
```

And then, for calculating the final fee:

```text
final_fee = inclusion_fee + tip
```

In the first formula, the `targeted_fee_adjustment` is a multiplier that can tune the final fee based on the network’s congestion.

- The `base_fee` derived from the base weight covers inclusion overhead like signature verification.
- The `length_fee` is a per-byte fee that is multiplied by the length of the encoded extrinsic.
- The `weight_fee` fee is calculated using two parameters:
  - The `ExtrinsicBaseWeight` that is declared in the runtime and applies to all extrinsics.
  - The `#[pallet::weight]` annotation that accounts for an extrinsic's complexity.

To convert the weight to `Currency`, the runtime must define a `WeightToFee` struct that implements a conversion function, [`Convert<Weight,Balance>`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/struct.Pallet.html#method.weight_to_fee).

Note that the extrinsic sender is charged the inclusion fee before the extrinsic is invoked. The fee is deducted from the sender's balance even if the transaction fails upon execution.

### Accounts with an Insufficient Balance

If an account does not have a sufficient balance to pay the inclusion fee and remain alive—that is, enough to pay the inclusion fee and maintain the minimum existential deposit—then you should ensure the transaction is canceled so that no fee is deducted and the transaction does not begin execution.

The Polkadot SDK doesn't enforce this rollback behavior. However, this scenario would be rare because the transaction queue and block-making logic perform checks to prevent it before adding an extrinsic to a block.

### Fee Multipliers

The inclusion fee formula always results in the same fee for the same input. However, weight can be dynamic and—based on how [`WeightToFee`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.WeightToFee) is defined—the final fee can include some degree of variability.
The Transaction Payment pallet provides the [`FeeMultiplierUpdate`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/pallet/trait.Config.html#associatedtype.FeeMultiplierUpdate) configurable parameter to account for this variability.

The Polkadot network inspires the default update function and implements a targeted adjustment in which a target saturation level of block weight is defined. If the previous block is more saturated, the fees increase slightly. Similarly, if the last block has fewer transactions than the target, fees are decreased by a small amount. For more information about fee multiplier adjustments, see the [Web3 Research Page](https://research.web3.foundation/Polkadot/overview/token-economics#relay-chain-transaction-fees-and-per-block-transaction-limits).

## Transactions with Special Requirements

Inclusion fees must be computable before execution and can only represent fixed logic. Some transactions warrant limiting resources with other strategies. For example:

- Bonds are a type of fee that might be returned or slashed after some on-chain event. For example, you might want to require users to place a bond to participate in a vote. The bond might then be returned at the end of the referendum or slashed if the voter attempted malicious behavior.
- Deposits are fees that might be returned later. For example, you might require users to pay a deposit to execute an operation that uses storage. The user’s deposit could be returned if a subsequent operation frees up storage.
- Burn operations are used to pay for a transaction based on its internal logic. For example, a transaction might burn funds from the sender if the transaction creates new storage items to pay for the increased state size.
- Limits enable you to enforce constant or configurable limits on specific operations. For example, the default [Staking pallet](https://github.com/paritytech/polkadot-sdk/tree/polkadot-stable2512-3/substrate/frame/staking) only allows nominators to nominate 16 validators to limit the complexity of the validator election process.

It is important to note that if you query the chain for a transaction fee, it only returns the inclusion fee.

## Default Weight Annotations

All dispatchable functions in the Polkadot SDK must specify a weight. The way of doing that is using the annotation-based system that lets you combine fixed values for database read/write weight and/or fixed values based on benchmarks. The most basic example would look like this:

```rust
#[pallet::weight(100_000)]
fn my_dispatchable()
```

Note that the [`ExtrinsicBaseWeight`](https://crates.parity.io/frame_support/weights/constants/struct.ExtrinsicBaseWeight.html) is automatically added to the declared weight to account for the costs of simply including an empty extrinsic into a block.

### Weights and Database Read/Write Operations

To make weight annotations independent of the deployed database backend, they are defined as a constant and then used in the annotations when expressing database accesses performed by the dispatchable:

```rust
#[pallet::weight(T::DbWeight::get().reads_writes(1, 2) + 20_000)]
fn my_dispatchable()
```

This dispatchable allows one database to read and two to write, in addition to other things that add the additional 20,000. Database access is generally every time a value declared inside the [`#[pallet::storage]`](https://paritytech.github.io/polkadot-sdk/master/frame_support/pallet_macros/attr.storage.html) block is accessed. However, unique accesses are counted because after a value is accessed, it is cached, and reaccessing it does not result in a database operation. That is:

- Multiple reads of the exact value count as one read.
- Multiple writes of the exact value count as one write.
- Multiple reads of the same value, followed by a write to that value, count as one read and one write.
- A write followed by a read-only counts as one write.

### Dispatch Classes

Dispatches are broken into three classes:

- Normal
- Operational
- Mandatory

If a dispatch is not defined as `Operational` or `Mandatory` in the weight annotation, the dispatch is identified as `Normal` by default. You can specify that the dispatchable uses another class like this:

```rust
#[pallet::dispatch((DispatchClass::Operational))]
fn my_dispatchable()
```

This tuple notation also allows you to specify a final argument determining whether the user is charged based on the annotated weight. If you don't specify otherwise, `Pays::Yes` is assumed:

```rust
#[pallet::dispatch(DispatchClass::Normal, Pays::No)]
fn my_dispatchable()
```

#### Normal Dispatches

Dispatches in this class represent normal user-triggered transactions. These types of dispatches only consume a portion of a block's total weight limit. For information about the maximum portion of a block that can be consumed for normal dispatches, see [`AvailableBlockRatio`](https://paritytech.github.io/polkadot-sdk/master/frame_system/limits/struct.BlockLength.html). Normal dispatches are sent to the transaction pool.

#### Operational Dispatches

Unlike normal dispatches, which represent the usage of network capabilities, operational dispatches are those that provide network capabilities. Operational dispatches can consume the entire weight limit of a block. They are not bound by the [`AvailableBlockRatio`](https://paritytech.github.io/polkadot-sdk/master/frame_system/limits/struct.BlockLength.html). Dispatches in this class are given maximum priority and are exempt from paying the [`length_fee`](https://docs.rs/pallet-transaction-payment/latest/pallet_transaction_payment/).

#### Mandatory Dispatches

Mandatory dispatches are included in a block even if they cause the block to surpass its weight limit. You can only use the mandatory dispatch class for inherent transactions that the block author submits. This dispatch class is intended to represent functions in the block validation process. Because these dispatches are always included in a block regardless of the function weight, the validation process must prevent malicious nodes from abusing the function to craft valid but impossibly heavy blocks. You can typically accomplish this by ensuring that:

- The operation performed is always light.
- The operation can only be included in a block once.

To make it more difficult for malicious nodes to abuse mandatory dispatches, they cannot be included in blocks that return errors. This dispatch class serves the assumption that it is better to allow an overweight block to be created than not to allow any block to be created at all.

### Dynamic Weights

In addition to purely fixed weights and constants, the weight calculation can consider the input arguments of a dispatchable. The weight should be trivially computable from the input arguments with some basic arithmetic:

```rust
use frame_support:: {
    dispatch:: {
        DispatchClass::Normal,
        Pays::Yes,
    },
   weights::Weight,
};

#[pallet::weight(FunctionOf(
  |args: (&Vec<User>,)| args.0.len().saturating_mul(10_000),
  )
]
fn handle_users(origin, calls: Vec<User>)
```

## Post Dispatch Weight Correction

Depending on the execution logic, a dispatchable function might consume less weight than was prescribed pre-dispatch. To correct weight, the function declares a different return type and returns its actual weight:

```rust
#[pallet::weight(10_000 + 500_000_000)]
fn expensive_or_cheap(input: u64) -> DispatchResultWithPostInfo {
    let was_heavy = do_calculation(input);

    if (was_heavy) else {
        // Return the actual weight consumed.
        Ok(Some(10_000).into())
    }
}
```

## Custom Fees

You can also define custom fee systems through custom weight functions or inclusion fee functions.

### Custom Weights

Instead of using the default weight annotations, you can create a custom weight calculation type using the weights module. The custom weight calculation type must implement the following traits:

- [`WeighData<T>`](https://crates.parity.io/frame_support/weights/trait.WeighData.html) to determine the weight of the dispatch.
- [`ClassifyDispatch<T>`](https://crates.parity.io/frame_support/weights/trait.ClassifyDispatch.html) to determine the class of the dispatch.
- [`PaysFee<T>`](https://crates.parity.io/frame_support/weights/trait.PaysFee.html) to determine whether the sender of the dispatch pays fees.
 
The Polkadot SDK then bundles the output information of the three traits into the [`DispatchInfo`](https://paritytech.github.io/polkadot-sdk/master/frame_support/dispatch/struct.DispatchInfo.html) struct and provides it by implementing the [`GetDispatchInfo`](https://docs.rs/frame-support/latest/frame_support/dispatch/trait.GetDispatchInfo.html) for all `Call` variants and opaque extrinsic types. This is used internally by the System and Executive modules.

`ClassifyDispatch`, `WeighData`, and `PaysFee` are generic over T, which gets resolved into the tuple of all dispatch arguments except for the origin. The following example illustrates a struct that calculates the weight as `m * len(args)`, where `m` is a given multiplier and args is the concatenated tuple of all dispatch arguments. In this example, the dispatch class is `Operational` if the transaction has more than 100 bytes of length in arguments and will pay fees if the encoded length exceeds 10 bytes.

```rust
struct LenWeight(u32);
impl<T> WeighData<T> for LenWeight {
    fn weigh_data(&self, target: T) -> Weight {
        let multiplier = self.0;
        let encoded_len = target.encode().len() as u32;
        multiplier * encoded_len
    }
}

impl<T> ClassifyDispatch<T> for LenWeight {
    fn classify_dispatch(&self, target: T) -> DispatchClass {
        let encoded_len = target.encode().len() as u32;
        if encoded_len > 100 {
            DispatchClass::Operational
        } else {
            DispatchClass::Normal
        }
    }
}

impl<T> PaysFee<T> {
    fn pays_fee(&self, target: T) -> Pays {
        let encoded_len = target.encode().len() as u32;
        if encoded_len > 10 {
            Pays::Yes
        } else {
            Pays::No
        }
    }
}
```

A weight calculator function can also be coerced to the final type of the argument instead of defining it as a vague type that can be encoded. The code would roughly look like this:

```rust
struct CustomWeight;
impl WeighData<(&u32, &u64)> for CustomWeight {
    fn weigh_data(&self, target: (&u32, &u64)) -> Weight {
        ...
    }
}

// given a dispatch:
#[pallet::call]
impl<T: Config<I>, I: 'static> Pallet<T, I> {
    #[pallet::weight(CustomWeight)]
    fn foo(a: u32, b: u64)
}
```

In this example, the `CustomWeight` can only be used in conjunction with a dispatch with a particular signature `(u32, u64)`, as opposed to `LenWeight`, which can be used with anything because there aren't any assumptions about `<T>`.

#### Custom Inclusion Fee

The following example illustrates how to customize your inclusion fee. You must configure the appropriate associated types in the respective module.

```rust
// Assume this is the balance type
type Balance = u64;

// Assume we want all the weights to have a `100 + 2 * w` conversion to fees
struct CustomWeightToFee;
impl WeightToFee<Weight, Balance> for CustomWeightToFee {
    fn convert(w: Weight) -> Balance {
        let a = Balance::from(100);
        let b = Balance::from(2);
        let w = Balance::from(w);
        a + b * w
    }
}

parameter_types! {
    pub const ExtrinsicBaseWeight: Weight = 10_000_000;
}

impl frame_system::Config for Runtime {
    type ExtrinsicBaseWeight = ExtrinsicBaseWeight;
}

parameter_types! {
    pub const TransactionByteFee: Balance = 10;
}

impl transaction_payment::Config {
    type TransactionByteFee = TransactionByteFee;
    type WeightToFee = CustomWeightToFee;
    type FeeMultiplierUpdate = TargetedFeeAdjustment<TargetBlockFullness>;
}

struct TargetedFeeAdjustment<T>(sp_std::marker::PhantomData<T>);
impl<T: Get<Perquintill>> WeightToFee<Fixed128, Fixed128> for TargetedFeeAdjustment<T> {
    fn convert(multiplier: Fixed128) -> Fixed128 {
        // Don't change anything. Put any fee update info here.
        multiplier
    }
}
```

## Additional Resources

You now know the weight system, how it affects transaction fee computation, and how to specify weights for your dispatchable calls. The next step is determining the correct weight for your dispatchable operations. You can use Substrate benchmarking functions and frame-benchmarking calls to test your functions with different parameters and empirically determine the proper weight in their worst-case scenarios.

- [Benchmark](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/benchmark-pallet/)
- [`SignedExtension`](https://paritytech.github.io/polkadot-sdk/master/sp_runtime/traits/trait.SignedExtension.html)
- [Custom weights for the Example pallet](https://github.com/paritytech/polkadot-sdk/blob/polkadot-stable2512-3/substrate/frame/examples/basic/src/weights.rs)
- [Web3 Foundation Research](https://research.web3.foundation/Polkadot/overview/token-economics#relay-chain-transaction-fees-and-per-block-transaction-limits)


---

Page Title: Transfer Assets Between Parachains

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-parachains.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-parachains/
- Summary: A step-by-step guide to using the ParaSpell XCM SDK to build, verify, and execute a transfer from one Parachain to another.
- Word Count: 2106; Token Estimate: 4784
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:c683dceaef61daabacf90f0b454a7d4cfd22515110f81fe542316faffb490d42

# Transfer Assets Between Parachains

## Introduction

This guide walks you through transferring tokens between two parachains using the [ParaSpell XCM SDK](/polkadot-mkdocs/reference/tools/paraspell/). This example utilizes [Asset Hub](/polkadot-mkdocs/reference/polkadot-hub/#asset-management) and the [People Chain](/polkadot-mkdocs/reference/polkadot-hub/#people-chain). However, the same approach can be applied to transfers between other parachains.

For development purposes, this guide will use the [Polkadot TestNet](/polkadot-mkdocs/smart-contracts/connect/#networks-details), so the transferred token will be PAS.

In this guide, you will:

- Build an XCM transfer transaction using ParaSpell XCM SDK.
- Perform a dry run to validate the transfer.
- Verify the [Existential Deposit (ED)](/polkadot-mkdocs/reference/glossary/#existential-deposit) requirement on the destination chain.
- Retrieve information regarding the transfer, along with fee estimates.
- Submit the transaction.

### Prerequisites

Before you begin, ensure you have the following:

- Knowledge of the [fundamentals of Polkadot](/polkadot-mkdocs/parachains/get-started/).
- Basic understanding of [XCM](/polkadot-mkdocs/parachains/interoperability/get-started/).
- Basic familiarity with JavaScript or TypeScript.
- Installed [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), a JavaScript and TypeScript package manager.

## Initialize Your Project

Create the project folder:

```bash
mkdir paraspell-transfer && \
cd paraspell-transfer
```

Initialize the JavaScript project:

```bash
npm init -y
```

Install dev dependencies:

```bash
npm install --save-dev @types/node@^22.12.0 tsx@^4.20.6 typescript@^5.7.3
```

Install the required dependencies:

```bash
npm install --save @paraspell/sdk@12.8.8 polkadot-api@1.23.3 @polkadot-labs/hdkd-helpers@0.0.28 @polkadot-labs/hdkd@0.0.27
```

Now add the following setup code to `index.ts`:

```ts title="index.ts"
import { Builder, hasDryRunSupport } from '@paraspell/sdk';
import {
  entropyToMiniSecret,
  mnemonicToEntropy,
  ss58Address,
} from '@polkadot-labs/hdkd-helpers';
import { getPolkadotSigner } from 'polkadot-api/signer';
import { sr25519CreateDerive } from '@polkadot-labs/hdkd';
import { inspect } from 'util';

// PAS token has 10 decimals
const PAS_UNITS = 10_000_000_000n;

const SEED_PHRASE =
  'INSERT_YOUR_SEED_PHRASE';

// Create Sr25519 signer from mnemonic
function getSigner()

const RECIPIENT_ADDRESS = ss58Address(getSigner().publicKey);
const SENDER_ADDRESS = ss58Address(getSigner().publicKey);
```

Replace the `INSERT_YOUR_SEED_PHRASE` with the seed phrase from your Polkadot development account.

Be sure to fund this account with some PAS tokens on the Paseo Asset Hub using the [Polkadot Faucet](https://faucet.polkadot.io/?parachain=1000).

!!!warning "Security Warning"
    Never commit your mnemonic phrase to production code. Use environment variables or secure key management systems.

## Build a Token Transfer Transaction

The next step is to build the transaction that you intend to execute.

In this example, you will transfer 10 PAS tokens from Paseo's Asset Hub to Paseo's People Chain system parachain.

Add the ParaSpell transaction code to your `index.ts` file:

```ts title="index.ts"
async function transfer())
    .address(RECIPIENT_ADDRESS)
    .build();

  console.log('Built transaction:', inspect(tx, { colors: true, depth: null }));

  const result = await tx.signAndSubmit(signer);
  console.log(inspect(result, { colors: true, depth: null }));
  process.exit(0);
}
```

Do not execute it just yet. You will perform a dry run of this transaction first to ensure it works as expected.

## Perform a Dry Run

Dry runs simulate the transaction without broadcasting it, allowing you to confirm success in advance.

Add the following dry run code to your `index.ts` script:

```ts title="index.ts"
async function dryRunTransfer()

  const tx = await Builder()
    .from('AssetHubPaseo')
    .to('PeoplePaseo')
    .currency({
      symbol: 'PAS',
      amount: 10n * PAS_UNITS,
    })
    .address(RECIPIENT_ADDRESS)
    .senderAddress(SENDER_ADDRESS)
    .dryRun();

  console.log(inspect(tx, { colors: true, depth: null }));
  process.exit(0);
}

dryRunTransfer();
```
Run the script using the following command:

```bash
npx tsx index.ts
```

The result of the dry run will look similar to the following example output:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>{
  failureReason: undefined,
  failureChain: undefined,
  origin: {
    success: true,
    fee: 17965000n,
    currency: 'PAS',
    asset: {
      symbol: 'PAS',
      isNative: true,
      decimals: 10,
      existentialDeposit: '100000000',
      location: { parents: 1, interior: { Here: null } },
      isFeeAsset: true,
      amount: 100000000000n
    },
    weight: undefined,
    forwardedXcms: [
      {
        type: 'V3',
        value: {
          parents: 1,
          interior: { type: 'X1', value: { type: 'Parachain', value: 1004 } }
        }
      },
      [
        {
          type: 'V3',
          value: [
            {
              type: 'ReceiveTeleportedAsset',
              value: [
                {
                  id: {
                    type: 'Concrete',
                    value: {
                      parents: 1,
                      interior: { type: 'Here', value: undefined }
                    }
                  },
                  fun: { type: 'Fungible', value: 100000000000n }
                }
              ]
            },
            { type: 'ClearOrigin', value: undefined },
            {
              type: 'BuyExecution',
              value: {
                fees: {
                  id: {
                    type: 'Concrete',
                    value: {
                      parents: 1,
                      interior: { type: 'Here', value: undefined }
                    }
                  },
                  fun: { type: 'Fungible', value: 100000000000n }
                },
                weight_limit: { type: 'Unlimited', value: undefined }
              }
            },
            {
              type: 'DepositAsset',
              value: {
                assets: {
                  type: 'Wild',
                  value: { type: 'AllCounted', value: 1 }
                },
                beneficiary: {
                  parents: 0,
                  interior: {
                    type: 'X1',
                    value: {
                      type: 'AccountId32',
                      value: {
                        network: undefined,
                        id: FixedSizeBinary {
                          asText: [Function (anonymous)],
                          asHex: [Function (anonymous)],
                          asOpaqueHex: [Function (anonymous)],
                          asBytes: [Function (anonymous)],
                          asOpaqueBytes: [Function (anonymous)]
                        }
                      }
                    }
                  }
                }
              }
            },
            {
              type: 'SetTopic',
              value: FixedSizeBinary {
                asText: [Function (anonymous)],
                asHex: [Function (anonymous)],
                asOpaqueHex: [Function (anonymous)],
                asBytes: [Function (anonymous)],
                asOpaqueBytes: [Function (anonymous)]
              }
            }
          ]
        }
      ]
    ],
    destParaId: 1004
  },
  assetHub: undefined,
  bridgeHub: undefined,
  destination: {
    success: true,
    fee: 817598n,
    currency: 'PAS',
    asset: {
      symbol: 'PAS',
      isNative: true,
      decimals: 10,
      existentialDeposit: '1000000000',
      location: { parents: 1, interior: { Here: null } },
      isFeeAsset: true
    },
    weight: { refTime: 176858000n, proofSize: 0n },
    forwardedXcms: [],
    destParaId: undefined
  },
  hops: []
}</span>
</div>
## Verify the Existential Deposit

Check if the recipient account meets the [Existential Deposit (ED)](/polkadot-mkdocs/reference/glossary/#existential-deposit) requirement before sending by using [`verifyEdOnDestination`](https://paraspell.github.io/docs/sdk/xcmUtils.html#verify-ed-on-destination):

```ts title="index.ts"
async function verifyED())
    .address(RECIPIENT_ADDRESS)
    .senderAddress(SENDER_ADDRESS)
    .verifyEdOnDestination();

  console.log(`ED verification ${isValid ? 'successful' : 'failed'}.`);
  process.exit(0);
}

verifyED();
```
Comment out the `dryRunTransfer()` function so that it is not executed again. Then, execute the `verifyED()` by running the following command:

```bash
npx tsx index.ts
```

After that, you will get output confirming the ED which will look similar to the following:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>ED verification successful.</span>
</div>
## Get Transfer Info and Fee Estimates

Before sending an XCM transaction, it is helpful to estimate the fees associated with executing and delivering the cross-chain message.

ParaSpell has a helpful function for this: [`getTransferInfo()`](https://paraspell.github.io/docs/sdk/xcmUtils.html#xcm-transfer-info). This function returns an estimate of the associated XCM fees, along with the account's balance before and after the fees are paid.

```ts title="index.ts"
async function XcmTransferInfo())
    .address(RECIPIENT_ADDRESS)
    .senderAddress(SENDER_ADDRESS)
    .getTransferInfo();

  console.log('Transfer Info:', info);
  process.exit(0);
}

XcmTransferInfo();
```

Comment out the `verifyED()` function so it doesn't execute again. Then, execute the `XcmTransferInfo()` function by running the following command:

```bash
npx tsx index.ts
```

You will see all the information for your transfer similar to the following example:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>Transfer Info: {
  chain: {
    origin: "AssetHubPaseo",
    destination: "PeoplePaseo",
    ecosystem: "PAS",
  },
  origin: {
    selectedCurrency: {
      sufficient: true,
      balance: 9799197260420n,
      balanceAfter: 9699197260420n,
      currencySymbol: "PAS",
      asset: [Object ...],
      existentialDeposit: 100000000n,
    },
    xcmFee: {
      sufficient: true,
      fee: 17965000n,
      balance: 9799197260420n,
      balanceAfter: 9799179295420n,
      currencySymbol: "PAS",
      asset: [Object ...],
    },
  },
  assetHub: undefined,
  bridgeHub: undefined,
  hops: [],
  destination: {
    receivedCurrency: {
      sufficient: true,
      receivedAmount: 99999182402n,
      balance: 199998364804n,
      balanceAfter: 299997547206n,
      currencySymbol: "PAS",
      asset: [Object ...],
      existentialDeposit: 1000000000n,
    },
    xcmFee: {
      fee: 817598n,
      balance: 199998364804n,
      balanceAfter: 299997547206n,
      currencySymbol: "PAS",
      asset: [Object ...],
    },
  },
}</span>
</div>
Now that you have:

- Completed a successful dry run of the transaction.
- Verified the existential deposit on the recipient account.
- Obtained an estimate of the associated XCM fees.

You can execute the transfer function by adding the following function call:

```typescript title="index.ts"
transfer();
```

Comment out the `XcmTransferInfo()` function so it doesn't execute again. Then, execute the transfer by running the following command: 

```bash
npx tsx index.ts
```

Your `transfer` function will submit the transaction, and you will get the following output:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>...</span>
  <span data-ty>Built transaction: {
  getPaymentInfo: [AsyncFunction: getPaymentInfo],
  getEstimatedFees: [AsyncFunction: getEstimatedFees],
  decodedCall: {
    type: 'PolkadotXcm',
    value: {
      type: 'limited_teleport_assets',
      value: {
        dest: {
          type: 'V5',
          value: {
            parents: 1,
            interior: { type: 'X1', value: { type: 'Parachain', value: 1004 } }
          }
        },
        beneficiary: {
          type: 'V5',
          value: {
            parents: 0,
            interior: {
              type: 'X1',
              value: {
                type: 'AccountId32',
                value: {
                  network: undefined,
                  id: FixedSizeBinary {
                    asText: [Function (anonymous)],
                    asHex: [Function (anonymous)],
                    asOpaqueHex: [Function (anonymous)],
                    asBytes: [Function (anonymous)],
                    asOpaqueBytes: [Function (anonymous)]
                  }
                }
              }
            }
          }
        },
        assets: {
          type: 'V5',
          value: [
            {
              id: { parents: 1, interior: { type: 'Here', value: null } },
              fun: { type: 'Fungible', value: 100000000000n }
            }
          ]
        },
        fee_asset_item: 0,
        weight_limit: { type: 'Unlimited' }
      }
    }
  },
  getEncodedData: [Function: getEncodedData],
  sign: [Function: sign],
  signSubmitAndWatch: [Function: signSubmitAndWatch],
  signAndSubmit: [Function: signAndSubmit]
}</span>
</div>
Once the transaction is successfully included in a block, you will see the recipient's account balance updated, and you will receive output similar to the following.

???- code "Successful Transaction Submission"
    This output will be returned once the transaction has been successfully included in a block.

    <div id="termynal" data-termynal>
      <span data-ty>...</span>
      <span data-ty>{
      txHash: '0x6fbecc0b284adcff46ab39872659c2567395c865adef5f8cbea72f25b6042609',
      block: {
        index: 2,
        number: 2524809,
        hash: '0xa39a96d5921402c6e8f67e48b8395d6b21382c72d4d30f8497a0e9f890bc0d4c'
      },
      ok: true,
      events: [
        {
          type: 'Balances',
          value: {
            type: 'Withdraw',
            value: {
              who: '15DMtB5BDCJqw4uZtByTWXGqViAVx7XjRsxWbTH5tfrHLe8j',
              amount: 15668864n
            }
          },
          topics: []
        },
        {
          type: 'Balances',
          value: {
            type: 'Burned',
            value: {
              who: '15DMtB5BDCJqw4uZtByTWXGqViAVx7XjRsxWbTH5tfrHLe8j',
              amount: 100000000000n
            }
          },
          topics: []
        },
        {
          type: 'PolkadotXcm',
          value: {
            type: 'Attempted',
            value: {
              outcome: {
                type: 'Complete',
                value: { used: { ref_time: 190990000n, proof_size: 3593n } }
              }
            }
          },
          topics: []
        },
        {
          type: 'Balances',
          value: {
            type: 'Burned',
            value: {
              who: '15DMtB5BDCJqw4uZtByTWXGqViAVx7XjRsxWbTH5tfrHLe8j',
              amount: 304850000n
            }
          },
          topics: []
        },
        {
          type: 'Balances',
          value: {
            type: 'Minted',
            value: {
              who: '14xmwinmCEz6oRrFdczHKqHgWNMiCysE2KrA4jXXAAM1Eogk',
              amount: 304850000n
            }
          },
          topics: []
        },
        {
          type: 'PolkadotXcm',
          value: {
            type: 'FeesPaid',
            value: {
              paying: {
                parents: 0,
                interior: {
                  type: 'X1',
                  value: {
                    type: 'AccountId32',
                    value: {
                      network: { type: 'Polkadot', value: undefined },
                      id: FixedSizeBinary {
                        asText: [Function (anonymous)],
                        asHex: [Function (anonymous)],
                        asOpaqueHex: [Function (anonymous)],
                        asBytes: [Function (anonymous)],
                        asOpaqueBytes: [Function (anonymous)]
                      }
                    }
                  }
                }
              },
              fees: [
                {
                  id: {
                    parents: 1,
                    interior: { type: 'Here', value: undefined }
                  },
                  fun: { type: 'Fungible', value: 304850000n }
                }
              ]
            }
          },
          topics: []
        },
        {
          type: 'XcmpQueue',
          value: {
            type: 'XcmpMessageSent',
            value: {
              message_hash: FixedSizeBinary {
                asText: [Function (anonymous)],
                asHex: [Function (anonymous)],
                asOpaqueHex: [Function (anonymous)],
                asBytes: [Function (anonymous)],
                asOpaqueBytes: [Function (anonymous)]
              }
            }
          },
          topics: []
        },
        {
          type: 'PolkadotXcm',
          value: {
            type: 'Sent',
            value: {
              origin: {
                parents: 0,
                interior: {
                  type: 'X1',
                  value: {
                    type: 'AccountId32',
                    value: {
                      network: { type: 'Polkadot', value: undefined },
                      id: FixedSizeBinary {
                        asText: [Function (anonymous)],
                        asHex: [Function (anonymous)],
                        asOpaqueHex: [Function (anonymous)],
                        asBytes: [Function (anonymous)],
                        asOpaqueBytes: [Function (anonymous)]
                      }
                    }
                  }
                }
              },
              destination: {
                parents: 1,
                interior: { type: 'X1', value: { type: 'Parachain', value: 1002 } }
              },
              message: [
                {
                  type: 'ReceiveTeleportedAsset',
                  value: [
                    {
                      id: {
                        parents: 1,
                        interior: { type: 'Here', value: undefined }
                      },
                      fun: { type: 'Fungible', value: 100000000000n }
                    }
                  ]
                },
                { type: 'ClearOrigin', value: undefined },
                {
                  type: 'BuyExecution',
                  value: {
                    fees: {
                      id: {
                        parents: 1,
                        interior: { type: 'Here', value: undefined }
                      },
                      fun: { type: 'Fungible', value: 100000000000n }
                    },
                    weight_limit: { type: 'Unlimited', value: undefined }
                  }
                },
                {
                  type: 'DepositAsset',
                  value: {
                    assets: {
                      type: 'Wild',
                      value: { type: 'AllCounted', value: 1 }
                    },
                    beneficiary: {
                      parents: 0,
                      interior: {
                        type: 'X1',
                        value: {
                          type: 'AccountId32',
                          value: {
                            network: undefined,
                            id: FixedSizeBinary {
                              asText: [Function (anonymous)],
                              asHex: [Function (anonymous)],
                              asOpaqueHex: [Function (anonymous)],
                              asBytes: [Function (anonymous)],
                              asOpaqueBytes: [Function (anonymous)]
                            }
                          }
                        }
                      }
                    }
                  }
                }
              ],
              message_id: FixedSizeBinary {
                asText: [Function (anonymous)],
                asHex: [Function (anonymous)],
                asOpaqueHex: [Function (anonymous)],
                asBytes: [Function (anonymous)],
                asOpaqueBytes: [Function (anonymous)]
              }
            }
          },
          topics: []
        },
        {
          type: 'Balances',
          value: {
            type: 'Deposit',
            value: {
              who: '13UVJyLgBASGhE2ok3TvxUfaQBGUt88JCcdYjHvUhvQkFTTx',
              amount: 15668864n
            }
          },
          topics: []
        },
        {
          type: 'TransactionPayment',
          value: {
            type: 'TransactionFeePaid',
            value: {
              who: '15DMtB5BDCJqw4uZtByTWXGqViAVx7XjRsxWbTH5tfrHLe8j',
              actual_fee: 15668864n,
              tip: 0n
            }
          },
          topics: []
        },
        {
          type: 'System',
          value: {
            type: 'ExtrinsicSuccess',
            value: {
              dispatch_info: {
                weight: { ref_time: 952851000n, proof_size: 13382n },
                class: { type: 'Normal', value: undefined },
                pays_fee: { type: 'Yes', value: undefined }
              }
            }
          },
          topics: []
        }
      ]
    }</span>
    </div>
After executing the transfer, check the account balance on [Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fsys.turboflakes.io%2Fasset-hub-paseo) for [Paseo's Asset Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fsys.turboflakes.io%2Fasset-hub-paseo#/accounts) and [Paseo's People Chain](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fsys.ibp.network%2Fpeople-paseo#/accounts).

You should see:

- The recipient account now has 10 more PAS tokens.
- The sender account has the transfer amount (10 PAS) + the fees amount debited from their account balance.

You have now successfully created and sent a cross-chain transfer using the ParaSpell XCM SDK!

???- code "Full Code"

    ```typescript title="index.ts"
    import { Builder, hasDryRunSupport } from '@paraspell/sdk';
    import {
      entropyToMiniSecret,
      mnemonicToEntropy,
      ss58Address,
    } from '@polkadot-labs/hdkd-helpers';
    import { getPolkadotSigner } from 'polkadot-api/signer';
    import { sr25519CreateDerive } from '@polkadot-labs/hdkd';
    import { inspect } from 'util';

    // PAS token has 10 decimals
    const PAS_UNITS = 10_000_000_000n;

    const SEED_PHRASE =
      'INSERT_YOUR_SEED_PHRASE';

    // Create Sr25519 signer from mnemonic
    function getSigner()

    const RECIPIENT_ADDRESS = ss58Address(getSigner().publicKey);
    const SENDER_ADDRESS = ss58Address(getSigner().publicKey);

    async function transfer())
        .address(RECIPIENT_ADDRESS)
        .build();

      console.log('Built transaction:', inspect(tx, { colors: true, depth: null }));

      const result = await tx.signAndSubmit(signer);
      console.log(inspect(result, { colors: true, depth: null }));
      process.exit(0);
    }

    async function dryRunTransfer()

      const tx = await Builder()
        .from('AssetHubPaseo')
        .to('PeoplePaseo')
        .currency({
          symbol: 'PAS',
          amount: 10n * PAS_UNITS,
        })
        .address(RECIPIENT_ADDRESS)
        .senderAddress(SENDER_ADDRESS)
        .dryRun();

      console.log(inspect(tx, { colors: true, depth: null }));
      process.exit(0);
    }

    dryRunTransfer();

    async function verifyED())
        .address(RECIPIENT_ADDRESS)
        .senderAddress(SENDER_ADDRESS)
        .verifyEdOnDestination();

      console.log(`ED verification ${isValid ? 'successful' : 'failed'}.`);
      process.exit(0);
    }

    verifyED();

    async function XcmTransferInfo())
        .address(RECIPIENT_ADDRESS)
        .senderAddress(SENDER_ADDRESS)
        .getTransferInfo();

      console.log('Transfer Info:', info);
      process.exit(0);
    }

    XcmTransferInfo();

    transfer();
    ```

## Next Steps

- **Read the docs**: Dive deeper into the features of the [ParaSpell XCM SDK](https://paraspell.github.io/docs/sdk/getting-started.html) documentation.

- **Learn about XCM**: Understand the underlying protocol by visiting the [Get Started with XCM](/polkadot-mkdocs/parachains/interoperability/get-started/) guide.


---

Page Title: Transfer Assets from Ethereum into Polkadot

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-into-polkadot.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/transfer-assets-into-polkadot/
- Summary: A step-by-step guide to bridging assets from Ethereum to Polkadot using the ParaSpell XCM SDK and Snowbridge.
- Word Count: 1816; Token Estimate: 3400
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:073f4bf8aaddce3caad307b8d5068efd1657e5ef512a039fd4cba5bbbd7ea228

# Transfer Assets from Ethereum into Polkadot

## Introduction

This guide walks you through transferring tokens from Ethereum to Polkadot using the [ParaSpell XCM SDK](/polkadot-mkdocs/reference/tools/paraspell/) and [Snowbridge](https://docs.snowbridge.network/). Snowbridge is a trustless, decentralized bridge integrated into the Polkadot protocol that enables secure asset transfers between the Ethereum and Polkadot ecosystems.

### How the Bridge Works

The following diagram shows the complete flow when bridging WETH from Ethereum to Polkadot:

```mermaid
sequenceDiagram
    participant User
    participant Ethereum
    participant Relayers
    participant Polkadot

    User->>Ethereum: 1. Approve & send tokens
    Ethereum->>Ethereum: Lock tokens
    Note over Relayers: ~30 min relay
    Relayers->>Polkadot: 2. Relay message
    Polkadot->>Polkadot: Verify & mint tokens
    Polkadot-->>User: 3. Tokens available
```

In this guide, you will:

- Set up an Ethereum development environment with the ParaSpell SDK
- Approve tokens for the Snowbridge Gateway contract
- Build and execute a bridge transfer from Ethereum to Polkadot Hub
- Monitor the transfer status

## Prerequisites

Before you begin, ensure you have the following:

- A basic understanding of [XCM](/polkadot-mkdocs/parachains/interoperability/get-started/).
- Familiarity with JavaScript/TypeScript and Ethereum development.
- [Node.js](https://nodejs.org/) v18 or higher and npm installed.
- An Ethereum wallet with WETH tokens. See [Prepare Tokens for Bridging](#prepare-tokens-for-bridging) for instructions to wrap ETH if needed.
- A Polkadot account to receive the bridged assets.

## Prepare Tokens for Bridging

To bridge ETH, you need to wrap it into WETH first. The WETH contract on Ethereum mainnet is:

```
0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
```

!!!note "Why WETH instead of ETH?"
    Snowbridge only supports ERC-20 tokens—it cannot bridge native ETH directly. The bridge uses a standardized token interface to lock assets on Ethereum and mint corresponding representations on Polkadot. Since native ETH doesn't conform to the ERC-20 standard, you must first wrap it into WETH (Wrapped ETH), an ERC-20-compliant token pegged 1:1 to ETH.

You can wrap ETH by:

1. Visiting the [WETH contract on Etherscan](https://etherscan.io/address/0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2#writeContract)
2. Connecting your wallet
3. Calling the `deposit` function with the amount of ETH you want to wrap

## Initialize Your Project

Follow these steps to setup and initialize your project:
1. Create the project folder using the following commands:

```bash
mkdir eth-to-polkadot-bridge && \
cd eth-to-polkadot-bridge
```

2. Initialize the JavaScript project:

```bash
npm init -y && npm pkg set type=module
```

Install dev dependencies:

```bash
npm install --save-dev @types/node tsx typescript
```

Sending assets from Ethereum to Polkadot requires the PJS version of the ParaSpell SDK. Install the necessary dependencies using the following command:

```bash
npm install @paraspell/sdk-pjs@12.8.8 @polkadot/api@16.5.6 @polkadot/types@16.5.6 ethers@6.16.0
```

Now add the following setup code to `index.ts`:

```ts title="index.ts"
import { EvmBuilder, getTokenBalance, approveToken, getBridgeStatus } from '@paraspell/sdk-pjs';
import { ethers } from 'ethers';

// Ethereum mainnet RPC endpoint (use your own API key)
const ETH_RPC = 'https://eth-mainnet.g.alchemy.com/v2/INSERT_API_KEY';

// WETH contract address on Ethereum mainnet
const WETH_ADDRESS = '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2';

// WETH has 18 decimals
const WETH_UNITS = 1_000_000_000_000_000_000n;

// Amount to bridge: 0.001 WETH
const AMOUNT = (WETH_UNITS / 1000n).toString();

// Your Polkadot address to receive the bridged tokens (SS58 format)
const RECIPIENT_ADDRESS = 'INSERT_YOUR_POLKADOT_ADDRESS';

// Connect to Ethereum mainnet
async function getProviderAndSigner();
}
```

Replace `INSERT_YOUR_POLKADOT_ADDRESS` with your Polkadot account address (SS58 format) that will receive the bridged tokens on Polkadot Hub.

!!!warning "Security Warning"
    Never commit private keys or seed phrases to production code. Use environment variables or secure key management systems.

## Approve Tokens for Bridging

Before bridging ERC-20 tokens, you must approve the Snowbridge Gateway contract to spend your tokens. The ParaSpell SDK provides the following helper functions for approving spending:

```ts title="index.ts"
async function approveTokens() = await getProviderAndSigner();

  // Check current WETH balance
  const balance = await getTokenBalance(signer, 'WETH');
  console.log(`Current WETH balance: ${balance}`);

  if (BigInt(balance) === 0n)`);
    process.exit(1);
  }

  // Approve the Snowbridge Gateway contract to spend WETH
  const { result: approveTx } = await approveToken(signer, BigInt(AMOUNT), 'WETH');
  console.log(`Approval transaction hash: ${approveTx.hash}`);

  // Wait for confirmation
  await approveTx.wait();
  console.log('Token approval confirmed!');
}
```

Run the approval script:

```bash
npx tsx index.ts
```

You will see output confirming the token balance and approval similar to the following:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>Current WETH balance: 0.05</span>
  <span data-ty>Approval transaction hash: 0x1a2b3c4d5e6f7890abcdef1234567890abcdef1234567890abcdef1234567890</span>
  <span data-ty>Token approval confirmed!</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
## Build and Execute the Bridge Transfer

Now build and execute the bridge transfer from Ethereum to Polkadot Hub:

```ts title="index.ts"
async function bridgeToPolkadot() = await getProviderAndSigner();

  console.log('Building bridge transaction...');
  console.log(`Bridging ${AMOUNT} WETH to Polkadot Hub`);
  console.log(`Recipient: ${RECIPIENT_ADDRESS}`);

  // Build and execute the bridge transfer
  // Note: 'AssetHubPolkadot' is the SDK identifier for Polkadot Hub
  const result = await EvmBuilder(provider)
    .to('AssetHubPolkadot')
    .currency({
      symbol: 'WETH',
      amount: AMOUNT,
    })
    .address(RECIPIENT_ADDRESS)
    .signer(signer)
    .build();

  console.log('Bridge transaction submitted!');
  console.log(`Transaction hash: ${result.response.hash}`);
  console.log('Transfer will arrive in approximately 30 minutes.');

  // Wait for Ethereum confirmation
  await result.response.wait();
  console.log('Ethereum transaction confirmed! Waiting for bridge relay...');
}
```

Comment out the `approveTokens()` function and run the transfer:

```bash
npx tsx index.ts
```

After submitting the transaction, you will see the Ethereum transaction hash included in the terminal output similar to the following:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx index.ts</span>
  <span data-ty>Building bridge transaction...</span>
  <span data-ty>Bridging 1000000000000000 WETH to Polkadot Hub</span>
  <span data-ty>Recipient: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY</span>
  <span data-ty>Bridge transaction submitted!</span>
  <span data-ty>Transaction hash: 0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890</span>
  <span data-ty>Transfer will arrive in approximately 30 minutes.</span>
  <span data-ty>Ethereum transaction confirmed! Waiting for bridge relay...</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
## Monitor the Transfer

Bridge transfers from Ethereum to Polkadot take approximately 30 minutes. You can monitor the transfer status in several ways:

- **Snowbridge App** - Visit [app.snowbridge.network](https://app.snowbridge.network/) to check the transfer history
- **Etherscan** - Track your Ethereum transaction on [etherscan.io](https://etherscan.io/)
- **Polkadot.js Apps** - Monitor incoming transfers on [Polkadot Hub](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpolkadot-asset-hub-rpc.polkadot.io#/accounts)

You can also check the bridge status programmatically:

```ts title="index.ts"
async function checkBridgeStatus()
```

## Supported Assets and Destinations

Snowbridge can bridge any ERC-20 token from Ethereum, but the destination chain must have that asset registered as a foreign asset to receive it.

!!!warning "Verify Foreign Assets"
    Foreign asset creation on Polkadot Hub is permissionless—anyone can register a foreign asset. Before bridging, verify that the foreign asset registered on Polkadot Hub corresponds to the legitimate token on Ethereum by checking the asset's multilocation and confirming the Ethereum contract address matches the official token contract.

You can query which assets are supported for a specific route using the ParaSpell SDK as follows:

```ts title="index.ts"
import { getSupportedAssets } from '@paraspell/sdk-pjs';

// Get assets supported for Ethereum → Polkadot Hub transfers
const supportedAssets = getSupportedAssets('Ethereum', 'AssetHubPolkadot');
console.log('Supported assets:', supportedAssets.map(a => a.symbol));
```

Running this script will return a list of supported assets in the terminal similar to the following:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx get-supported-assets.ts</span>
  <span data-ty>Supported assets: [</span>
  <span data-ty>  'WETH',    'WBTC',  'USDC',  'USDT',</span>
  <span data-ty>  'DAI',     'SHIB',  'LINK',  'AAVE',</span>
  <span data-ty>  'wstETH',  'tBTC',  'PEPE',  'EURC',</span>
  <span data-ty>  'MYTH',    'TRAC',  'KILT',  'DOT',</span>
  <span data-ty>  ... and more</span>
  <span data-ty>]</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
Common supported tokens on Polkadot Hub include:

| Token | Symbol | Description |
|-------|--------|-------------|
| Wrapped Ether | WETH | Ethereum's native token wrapped as ERC-20 |
| Wrapped Bitcoin | WBTC | Bitcoin wrapped as an ERC-20 token |
| Shiba Inu | SHIB | Popular ERC-20 meme token |

To transfer to a different destination, change the `.to()` parameter and verify the asset is supported using the following code:

```ts title="index.ts"
// Check if WETH is supported on Hydration
const hydrationAssets = getSupportedAssets('Ethereum', 'Hydration');
const wethSupported = hydrationAssets.some(a => a.symbol === 'WETH');

if (wethSupported))
    .address(recipientAddress)
    .signer(signer)
    .build();
}
```

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx tsx bridge-to-hydration.ts</span>
  <span data-ty>WETH supported on Hydration: true</span>
  <span data-ty>Building bridge transaction to Hydration...</span>
  <span data-ty>Bridge transaction submitted!</span>
  <span data-ty>Transaction hash: 0x1234...abcd</span>
  <span data-ty="input"><span class="file-path"></span></span>
</div>
???- code "Complete `index.ts` script"

    ```typescript title="index.ts"
    import { EvmBuilder, getTokenBalance, approveToken, getBridgeStatus } from '@paraspell/sdk-pjs';
    import { ethers } from 'ethers';

    // Ethereum mainnet RPC endpoint (use your own API key)
    const ETH_RPC = 'https://eth-mainnet.g.alchemy.com/v2/INSERT_API_KEY';

    // WETH contract address on Ethereum mainnet
    const WETH_ADDRESS = '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2';

    // WETH has 18 decimals
    const WETH_UNITS = 1_000_000_000_000_000_000n;

    // Amount to bridge: 0.001 WETH
    const AMOUNT = (WETH_UNITS / 1000n).toString();

    // Your Polkadot address to receive the bridged tokens (SS58 format)
    const RECIPIENT_ADDRESS = 'INSERT_YOUR_POLKADOT_ADDRESS';

    // Connect to Ethereum mainnet
    async function getProviderAndSigner();
    }

    async function approveTokens() = await getProviderAndSigner();

      // Check current WETH balance
      const balance = await getTokenBalance(signer, 'WETH');
      console.log(`Current WETH balance: ${balance}`);

      if (BigInt(balance) === 0n)`);
        process.exit(1);
      }

      // Approve the Snowbridge Gateway contract to spend WETH
      const { result: approveTx } = await approveToken(signer, BigInt(AMOUNT), 'WETH');
      console.log(`Approval transaction hash: ${approveTx.hash}`);

      // Wait for confirmation
      await approveTx.wait();
      console.log('Token approval confirmed!');
    }

    approveTokens();

    async function bridgeToPolkadot() = await getProviderAndSigner();

      console.log('Building bridge transaction...');
      console.log(`Bridging ${AMOUNT} WETH to Polkadot Hub`);
      console.log(`Recipient: ${RECIPIENT_ADDRESS}`);

      // Build and execute the bridge transfer
      // Note: 'AssetHubPolkadot' is the SDK identifier for Polkadot Hub
      const result = await EvmBuilder(provider)
        .to('AssetHubPolkadot')
        .currency({
          symbol: 'WETH',
          amount: AMOUNT,
        })
        .address(RECIPIENT_ADDRESS)
        .signer(signer)
        .build();

      console.log('Bridge transaction submitted!');
      console.log(`Transaction hash: ${result.response.hash}`);
      console.log('Transfer will arrive in approximately 30 minutes.');

      // Wait for Ethereum confirmation
      await result.response.wait();
      console.log('Ethereum transaction confirmed! Waiting for bridge relay...');
    }

    bridgeToPolkadot();

    async function checkBridgeStatus()

    checkBridgeStatus();
    ```

!!!info "Transfer Assets Out of Polkadot"

    To transfer assets out of Polkadot, you can use the ParaSpell XCM SDK to build and execute a bridge transfer from Polkadot to Ethereum. Check out the [Polkadot to Ethereum](https://paraspell.github.io/docs/sdk/xcmPallet.html#polkadot-ethereum-transfer) section of the ParaSpell XCM SDK documentation.


## Where to Go Next

- **External Documentation** - Dive deeper into the [ParaSpell XCM SDK](https://paraspell.github.io/docs/sdk/getting-started.html) and [Snowbridge](https://docs.snowbridge.network/) documentation resources.
- **Learn about XCM** - Understand the underlying protocol by visiting the [Get Started with XCM](/polkadot-mkdocs/parachains/interoperability/get-started/) guide


---

Page Title: XCM Fee Estimation

- Resolved Markdown: https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/estimate-xcm-fees.md
- Canonical (HTML): https://docs.polkadot.com/chain-interactions/send-transactions/interoperability/estimate-xcm-fees/
- Summary: This tutorial demonstrates how to estimate the fees for teleporting assets from the Polkadot Hub TestNet to the Paseo People Chain.
- Word Count: 2983; Token Estimate: 6030
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:d852f23f302c10ec8ba90ae3cac5f5470888fa55df27a6e4584ea1e314e7632d

# XCM Fee Estimation

## Introduction

When sending cross-chain messages, ensure that the transaction will be successful not only in the local chain but also in the destination chain and any intermediate chains.

Sending cross-chain messages requires estimating the fees for the operation. 

This tutorial will demonstrate how to dry-run and estimate the fees for teleporting assets from the Polkadot Hub TestNet to the Paseo People Chain.

## Fee Mechanism

There are three types of fees that can be charged when sending a cross-chain message:

- **Local execution fees**: Fees charged in the local chain for executing the message.
- **Delivery fees**: Fees charged for delivering the message to the destination chain.
- **Remote execution fees**: Fees charged in the destination chain for executing the message.

If there are multiple intermediate chains, delivery fees and remote execution fees will be charged for each one.

In this example, you will estimate the fees for teleporting assets from the Polkadot Hub to the Paseo People Chain. The fee structure will be as follows:

```mermaid
flowchart LR
    PolkadotHub[Polkadot Hub] -->|Delivery Fees| PeopleChain[Paseo People Chain]
    PolkadotHub -->|Local<br/>Execution<br/>Fees| PolkadotHub
    PeopleChain -->|Remote<br/>Execution<br/>Fees| PeopleChain
```

The overall fees are `local_execution_fees` + `delivery_fees` + `remote_execution_fees`.

## Environment Setup

First, you need to set up your environment:

1. Create a new directory and initialize the project:

    ```bash
    mkdir xcm-fee-estimation && \
    cd xcm-fee-estimation
    ```

2. Initialize the project:

    ```bash
    npm init -y
    ```

3. Install dev dependencies:

    ```bash
    npm install --save-dev @types/node@^22.12.0 ts-node@^10.9.2 typescript@^5.7.3
    ```

4. Install dependencies:

    ```bash
    npm install --save @polkadot-labs/hdkd@0.0.27 @polkadot-labs/hdkd-helpers@0.0.28 polkadot-api@1.23.3
    ```

5. Create TypeScript configuration:

    ```bash
    npx tsc --init
    ```

6. Generate the types for the Polkadot API for Paseo People Chain and Polkadot Hub TestNet:

    ```bash
    npx papi add polkadotHub -n paseo_asset_hub && \
    npx papi add paseoPeopleChain -w wss://people-paseo.rpc.amforc.com
    ```

    !!!info "Polkadot Hub"
        The `paseo_asset_hub` is the identifier for the Polkadot Hub TestNet.

7. Create a new file called `teleport-polkadot-hub-to-people-chain.ts`:

    ```bash
    touch teleport-polkadot-hub-to-people-chain.ts
    ```

8. Import the necessary modules. Add the following code to the `teleport-polkadot-hub-to-people-chain.ts` file:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
    import { polkadotHub, paseoPeopleChain } from '@polkadot-api/descriptors';
    import { createClient, FixedSizeBinary, Enum } from 'polkadot-api';
    import { getWsProvider } from 'polkadot-api/ws-provider/node';
    import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
    import {
      XcmVersionedLocation,
      XcmVersionedAssetId,
      XcmV3Junctions,
      XcmV3MultiassetFungibility,
      XcmVersionedXcm,
      XcmV5Instruction,
      XcmV5Junctions,
      XcmV5Junction,
      XcmV5AssetFilter,
      XcmV5WildAsset,
    } from '@polkadot-api/descriptors';
    ```

9. Define constants and a `main` function where you will implement all the logic:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
    // 1 PAS = 10^10 units
    const PAS_UNITS = 10_000_000_000n; // 1 PAS
    const PAS_CENTS = 100_000_000n; // 0.01 PAS

    // Polkadot Hub constants
    const POLKADOT_HUB_RPC_ENDPOINT = 'ws://localhost:8001';
    const POLKADOT_HUB_ACCOUNT = '15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5'; // Alice (Polkadot Hub)

    // People Chain destination
    const PEOPLE_CHAIN_RPC_ENDPOINT = 'ws://localhost:8000';
    const PEOPLE_CHAIN_PARA_ID = 1004;
    const PEOPLE_CHAIN_BENEFICIARY =
    async function main()
    ```

All the following code explained in the subsequent sections must be added inside the `main` function.

## Client and API Setup

Now you are ready to start implementing the logic for the fee estimation for the teleport you want to perform. In this step, you will create the client for the Polkadot Hub TestNet and generate the typed API to interact with the chain. Follow the steps below:

Create the API client. You will need to create a client for the Polkadot Hub TestNet:

```typescript title="teleport-polkadot-hub-to-people-chain.ts"
  // Connect to the Polkadot Hub parachain
  const polkadotHubClient = createClient(
    withPolkadotSdkCompat(getWsProvider(POLKADOT_HUB_RPC_ENDPOINT)),
  );

  // Get the typed API for Polkadot Hub
  const polkadotHubApi = polkadotHubClient.getTypedApi(polkadotHub);
```

Ensure that you replace the endpoint URLs with the actual WebSocket endpoints. This example uses local chopsticks endpoints, but you can use public endpoints or run local nodes.

## Create the XCM Message

Now, you can construct a proper XCM message using the new XCM V5 instructions for teleporting from Polkadot Hub TestNet to the People Chain:

```typescript title="teleport-polkadot-hub-to-people-chain.ts"
function createTeleportXcmToPeopleChain(paraId: number),
        fun: XcmV3MultiassetFungibility.Fungible(1n * PAS_UNITS), // 1 PAS
      },
    ]),
    // Pay local fees on Polkadot Hub in PAS
    XcmV5Instruction.PayFees({
      asset: {
        id: { parents: 1, interior: XcmV5Junctions.Here() },
        fun: XcmV3MultiassetFungibility.Fungible(10n * PAS_CENTS), // 0.01 PAS
      },
    }),
    // Send to People Chain parachain (parents:1, interior: X1(Parachain(paraId)))
    XcmV5Instruction.InitiateTransfer({
      destination: {
        parents: 1,
        interior: XcmV5Junctions.X1(XcmV5Junction.Parachain(paraId)),
      },
      remote_fees: Enum(
        'Teleport',
        XcmV5AssetFilter.Definite([
          {
            id: { parents: 1, interior: XcmV5Junctions.Here() },
            fun: XcmV3MultiassetFungibility.Fungible(10n * PAS_CENTS), // 0.01 PAS
          },
        ]),
      ),
      preserve_origin: false,
      remote_xcm: [
        XcmV5Instruction.DepositAsset({
          assets: XcmV5AssetFilter.Wild(XcmV5WildAsset.AllCounted(1)),
          beneficiary: {
            parents: 0,
            interior: XcmV5Junctions.X1(
              XcmV5Junction.AccountId32({
                network: undefined,
                id: FixedSizeBinary.fromAccountId32(PEOPLE_CHAIN_BENEFICIARY),
              }),
            ),
          },
        }),
      ],
      assets: [
        Enum('Teleport', XcmV5AssetFilter.Wild(XcmV5WildAsset.AllCounted(1))), // Send everything.
      ],
    }),
  ]);
}
```

## Fee Estimation Function

Below is a four-step breakdown of the logic needed to estimate the fees for the teleport.

First, you need to create the function that will estimate the fees for the teleport:

```typescript title="teleport-polkadot-hub-to-people-chain.ts"
async function estimateXcmFeesFromPolkadotHubToPeopleChain(
  xcm: any,
  polkadotHubApi: any,
)
```

1. **Local execution fees on Polkadot Hub**: Compute the XCM weight locally, then convert that weight to PAS using Polkadot Hub's view of PAS (`parents: 1, interior: Here`). Add the code to the function:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
      console.log('=== Fee Estimation Process (Polkadot Hub → People Chain) ===');

      // 1. LOCAL EXECUTION FEES on Polkadot Hub
      console.log('1. Calculating local execution fees on Polkadot Hub...');
      let localExecutionFees = 0n;

      const weightResult =
        await polkadotHubApi.apis.XcmPaymentApi.query_xcm_weight(xcm);
      if (weightResult.success)),
          );

        if (executionFeesResult.success) else {
          console.log(
            '✗ Failed to calculate local execution fees:',
            executionFeesResult.value,
          );
        }
      } else {
        console.log(
          '✗ Failed to query XCM weight on Polkadot Hub:',
          weightResult.value,
        );
      }
    ```

2. **Dry-run and delivery fees to People Chain**: Dry-run the XCM on Polkadot Hub to capture forwarded messages, locate the one targeting People Chain (`parents: 1, interior: Here`), and ask for delivery fees. Add the code to the function:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
      // 2. DELIVERY FEES + REMOTE EXECUTION FEES
      console.log('\n2. Calculating delivery and remote execution fees...');
      let deliveryFees = 0n;
      let remoteExecutionFees = 0n; // Skipped (People Chain descriptor not available)

      // Origin from Polkadot Hub perspective
      const origin = XcmVersionedLocation.V5({
        parents: 0,
        interior: XcmV5Junctions.X1(
          XcmV5Junction.AccountId32({
            id: FixedSizeBinary.fromAccountId32(POLKADOT_HUB_ACCOUNT),
            network: undefined,
          }),
        ),
      });

      // Dry run the XCM locally on Polkadot Hub
      const dryRunResult = await polkadotHubApi.apis.DryRunApi.dry_run_xcm(
        origin,
        xcm,
      );

      if (
        dryRunResult.success &&
        dryRunResult.value.execution_result.type === 'Complete'
      ) = dryRunResult.value;

        // Find the XCM message sent to People Chain (parents:1, interior: X1(Parachain(1004)))
        const peopleChainXcmEntry = forwardedXcms.find(
          ([location, _]: [any, any]) =>
            (location.type === 'V4' || location.type === 'V5') &&
            location.value.parents === 1 &&
            location.value.interior?.type === 'X1' &&
            location.value.interior.value?.type === 'Parachain' &&
            location.value.interior.value.value === PEOPLE_CHAIN_PARA_ID,
        );

        if (peopleChainXcmEntry) else {
            console.log('✗ Failed to calculate delivery fees:', deliveryFeesResult);
          }
    ```

3. **Remote execution fees on People Chain**: Connect to People Chain, recompute the forwarded XCM weight there, and convert weight to PAS (`parents: 0, interior: Here`). Add the code to the function:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
          // 3. REMOTE EXECUTION FEES on People Chain
          console.log('\n3. Calculating remote execution fees on People Chain');
          try {
            const peopleChainClient = createClient(
              withPolkadotSdkCompat(getWsProvider(PEOPLE_CHAIN_RPC_ENDPOINT)),
            );
            const peopleChainApi = peopleChainClient.getTypedApi(paseoPeopleChain);
            const remoteWeightResult =
              await peopleChainApi.apis.XcmPaymentApi.query_xcm_weight(remoteXcm);
            const remoteFeesResult =
              await peopleChainApi.apis.XcmPaymentApi.query_weight_to_asset_fee(
                remoteWeightResult.value as {
                  ref_time: bigint;
                  proof_size: bigint;
                },
                XcmVersionedAssetId.V4({
                  parents: 1,
                  interior: XcmV3Junctions.Here(),
                }),
              );
            peopleChainClient.destroy();
            remoteExecutionFees = remoteFeesResult.value as bigint;
            console.log(
              '✓ Remote execution fees:',
              remoteExecutionFees.toString(),
              'PAS units',
            );
          } catch (error)
        } else {
          console.log('✗ No XCM message found to People Chain');
        }
      } else {
        console.log('✗ Local dry run failed on Polkadot Hub:', dryRunResult.value);
      }
    ```

4. **Sum and return totals**: Aggregate all parts, print a short summary, and return a structured result. Add the code to the function:

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
      // 4. TOTAL FEES
      const totalFees = localExecutionFees + deliveryFees + remoteExecutionFees;

      console.log('\n=== Fee Summary (Polkadot Hub → People Chain) ===');
      console.log(
        'Local execution fees:',
        localExecutionFees.toString(),
        'PAS units',
      );
      console.log('Delivery fees:', deliveryFees.toString(), 'PAS units');
      console.log(
        'Remote execution fees:',
        remoteExecutionFees.toString(),
        'PAS units',
      );
      console.log('TOTAL FEES:', totalFees.toString(), 'PAS units');
      console.log(
        'TOTAL FEES:',
        (Number(totalFees) / Number(PAS_UNITS)).toFixed(4),
        'PAS',
      );

      return {
        localExecutionFees,
        deliveryFees,
        remoteExecutionFees,
        totalFees,
      };
    }
    ```

The full code for the fee estimation function is the following:

??? code "Fee Estimation Function"

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
    async function estimateXcmFeesFromPolkadotHubToPeopleChain(
      xcm: any,
      polkadotHubApi: any,
    )),
          );

        if (executionFeesResult.success) else {
          console.log(
            '✗ Failed to calculate local execution fees:',
            executionFeesResult.value,
          );
        }
      } else {
        console.log(
          '✗ Failed to query XCM weight on Polkadot Hub:',
          weightResult.value,
        );
      }

      // 2. DELIVERY FEES + REMOTE EXECUTION FEES
      console.log('\n2. Calculating delivery and remote execution fees...');
      let deliveryFees = 0n;
      let remoteExecutionFees = 0n; // Skipped (People Chain descriptor not available)

      // Origin from Polkadot Hub perspective
      const origin = XcmVersionedLocation.V5({
        parents: 0,
        interior: XcmV5Junctions.X1(
          XcmV5Junction.AccountId32({
            id: FixedSizeBinary.fromAccountId32(POLKADOT_HUB_ACCOUNT),
            network: undefined,
          }),
        ),
      });

      // Dry run the XCM locally on Polkadot Hub
      const dryRunResult = await polkadotHubApi.apis.DryRunApi.dry_run_xcm(
        origin,
        xcm,
      );

      if (
        dryRunResult.success &&
        dryRunResult.value.execution_result.type === 'Complete'
      ) = dryRunResult.value;

        // Find the XCM message sent to People Chain (parents:1, interior: X1(Parachain(1004)))
        const peopleChainXcmEntry = forwardedXcms.find(
          ([location, _]: [any, any]) =>
            (location.type === 'V4' || location.type === 'V5') &&
            location.value.parents === 1 &&
            location.value.interior?.type === 'X1' &&
            location.value.interior.value?.type === 'Parachain' &&
            location.value.interior.value.value === PEOPLE_CHAIN_PARA_ID,
        );

        if (peopleChainXcmEntry) else {
            console.log('✗ Failed to calculate delivery fees:', deliveryFeesResult);
          }

          // 3. REMOTE EXECUTION FEES on People Chain
          console.log('\n3. Calculating remote execution fees on People Chain');
          try {
            const peopleChainClient = createClient(
              withPolkadotSdkCompat(getWsProvider(PEOPLE_CHAIN_RPC_ENDPOINT)),
            );
            const peopleChainApi = peopleChainClient.getTypedApi(paseoPeopleChain);
            const remoteWeightResult =
              await peopleChainApi.apis.XcmPaymentApi.query_xcm_weight(remoteXcm);
            const remoteFeesResult =
              await peopleChainApi.apis.XcmPaymentApi.query_weight_to_asset_fee(
                remoteWeightResult.value as {
                  ref_time: bigint;
                  proof_size: bigint;
                },
                XcmVersionedAssetId.V4({
                  parents: 1,
                  interior: XcmV3Junctions.Here(),
                }),
              );
            peopleChainClient.destroy();
            remoteExecutionFees = remoteFeesResult.value as bigint;
            console.log(
              '✓ Remote execution fees:',
              remoteExecutionFees.toString(),
              'PAS units',
            );
          } catch (error)
        } else {
          console.log('✗ No XCM message found to People Chain');
        }
      } else {
        console.log('✗ Local dry run failed on Polkadot Hub:', dryRunResult.value);
      }

      // 4. TOTAL FEES
      const totalFees = localExecutionFees + deliveryFees + remoteExecutionFees;

      console.log('\n=== Fee Summary (Polkadot Hub → People Chain) ===');
      console.log(
        'Local execution fees:',
        localExecutionFees.toString(),
        'PAS units',
      );
      console.log('Delivery fees:', deliveryFees.toString(), 'PAS units');
      console.log(
        'Remote execution fees:',
        remoteExecutionFees.toString(),
        'PAS units',
      );
      console.log('TOTAL FEES:', totalFees.toString(), 'PAS units');
      console.log(
        'TOTAL FEES:',
        (Number(totalFees) / Number(PAS_UNITS)).toFixed(4),
        'PAS',
      );

      return {
        localExecutionFees,
        deliveryFees,
        remoteExecutionFees,
        totalFees,
      };
    }
    ```

## Complete Implementation

Now put it all together in the main function:

```typescript title="teleport-polkadot-hub-to-people-chain.ts"
async function main(),
    });

    console.log('\n=== Transaction Details ===');
    console.log('Transaction hex:', (await tx.getEncodedData()).asHex());
    console.log('Ready to submit!');
  } catch (error));
    }
  } finally {
    // Ensure client is always destroyed
    polkadotHubClient.destroy();
  }
}
```

## Full Code

The full code for the complete implementation is the following:

??? code "Teleport from Polkadot Hub to People Chain"

    ```typescript title="teleport-polkadot-hub-to-people-chain.ts"
    import { polkadotHub, paseoPeopleChain } from '@polkadot-api/descriptors';
    import { createClient, FixedSizeBinary, Enum } from 'polkadot-api';
    import { getWsProvider } from 'polkadot-api/ws-provider/node';
    import { withPolkadotSdkCompat } from 'polkadot-api/polkadot-sdk-compat';
    import {
      XcmVersionedLocation,
      XcmVersionedAssetId,
      XcmV3Junctions,
      XcmV3MultiassetFungibility,
      XcmVersionedXcm,
      XcmV5Instruction,
      XcmV5Junctions,
      XcmV5Junction,
      XcmV5AssetFilter,
      XcmV5WildAsset,
    } from '@polkadot-api/descriptors';

    // 1 PAS = 10^10 units
    const PAS_UNITS = 10_000_000_000n; // 1 PAS
    const PAS_CENTS = 100_000_000n; // 0.01 PAS

    // Polkadot Hub constants
    const POLKADOT_HUB_RPC_ENDPOINT = 'ws://localhost:8001';
    const POLKADOT_HUB_ACCOUNT = '15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5'; // Alice (Polkadot Hub)

    // People Chain destination
    const PEOPLE_CHAIN_RPC_ENDPOINT = 'ws://localhost:8000';
    const PEOPLE_CHAIN_PARA_ID = 1004;
    const PEOPLE_CHAIN_BENEFICIARY =
      '14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3'; // Bob (People Chain)

    // Create the XCM message for teleport (Polkadot Hub → People Chain)
    function createTeleportXcmToPeopleChain(paraId: number),
            fun: XcmV3MultiassetFungibility.Fungible(1n * PAS_UNITS), // 1 PAS
          },
        ]),
        // Pay local fees on Polkadot Hub in PAS
        XcmV5Instruction.PayFees({
          asset: {
            id: { parents: 1, interior: XcmV5Junctions.Here() },
            fun: XcmV3MultiassetFungibility.Fungible(10n * PAS_CENTS), // 0.01 PAS
          },
        }),
        // Send to People Chain parachain (parents:1, interior: X1(Parachain(paraId)))
        XcmV5Instruction.InitiateTransfer({
          destination: {
            parents: 1,
            interior: XcmV5Junctions.X1(XcmV5Junction.Parachain(paraId)),
          },
          remote_fees: Enum(
            'Teleport',
            XcmV5AssetFilter.Definite([
              {
                id: { parents: 1, interior: XcmV5Junctions.Here() },
                fun: XcmV3MultiassetFungibility.Fungible(10n * PAS_CENTS), // 0.01 PAS
              },
            ]),
          ),
          preserve_origin: false,
          remote_xcm: [
            XcmV5Instruction.DepositAsset({
              assets: XcmV5AssetFilter.Wild(XcmV5WildAsset.AllCounted(1)),
              beneficiary: {
                parents: 0,
                interior: XcmV5Junctions.X1(
                  XcmV5Junction.AccountId32({
                    network: undefined,
                    id: FixedSizeBinary.fromAccountId32(PEOPLE_CHAIN_BENEFICIARY),
                  }),
                ),
              },
            }),
          ],
          assets: [
            Enum('Teleport', XcmV5AssetFilter.Wild(XcmV5WildAsset.AllCounted(1))), // Send everything.
          ],
        }),
      ]);
    }

    async function estimateXcmFeesFromPolkadotHubToPeopleChain(
      xcm: any,
      polkadotHubApi: any,
    )),
          );

        if (executionFeesResult.success) else {
          console.log(
            '✗ Failed to calculate local execution fees:',
            executionFeesResult.value,
          );
        }
      } else {
        console.log(
          '✗ Failed to query XCM weight on Polkadot Hub:',
          weightResult.value,
        );
      }

      // 2. DELIVERY FEES + REMOTE EXECUTION FEES
      console.log('\n2. Calculating delivery and remote execution fees...');
      let deliveryFees = 0n;
      let remoteExecutionFees = 0n; // Skipped (People Chain descriptor not available)

      // Origin from Polkadot Hub perspective
      const origin = XcmVersionedLocation.V5({
        parents: 0,
        interior: XcmV5Junctions.X1(
          XcmV5Junction.AccountId32({
            id: FixedSizeBinary.fromAccountId32(POLKADOT_HUB_ACCOUNT),
            network: undefined,
          }),
        ),
      });

      // Dry run the XCM locally on Polkadot Hub
      const dryRunResult = await polkadotHubApi.apis.DryRunApi.dry_run_xcm(
        origin,
        xcm,
      );

      if (
        dryRunResult.success &&
        dryRunResult.value.execution_result.type === 'Complete'
      ) = dryRunResult.value;

        // Find the XCM message sent to People Chain (parents:1, interior: X1(Parachain(1004)))
        const peopleChainXcmEntry = forwardedXcms.find(
          ([location, _]: [any, any]) =>
            (location.type === 'V4' || location.type === 'V5') &&
            location.value.parents === 1 &&
            location.value.interior?.type === 'X1' &&
            location.value.interior.value?.type === 'Parachain' &&
            location.value.interior.value.value === PEOPLE_CHAIN_PARA_ID,
        );

        if (peopleChainXcmEntry) else {
            console.log('✗ Failed to calculate delivery fees:', deliveryFeesResult);
          }

          // 3. REMOTE EXECUTION FEES on People Chain
          console.log('\n3. Calculating remote execution fees on People Chain');
          try {
            const peopleChainClient = createClient(
              withPolkadotSdkCompat(getWsProvider(PEOPLE_CHAIN_RPC_ENDPOINT)),
            );
            const peopleChainApi = peopleChainClient.getTypedApi(paseoPeopleChain);
            const remoteWeightResult =
              await peopleChainApi.apis.XcmPaymentApi.query_xcm_weight(remoteXcm);
            const remoteFeesResult =
              await peopleChainApi.apis.XcmPaymentApi.query_weight_to_asset_fee(
                remoteWeightResult.value as {
                  ref_time: bigint;
                  proof_size: bigint;
                },
                XcmVersionedAssetId.V4({
                  parents: 1,
                  interior: XcmV3Junctions.Here(),
                }),
              );
            peopleChainClient.destroy();
            remoteExecutionFees = remoteFeesResult.value as bigint;
            console.log(
              '✓ Remote execution fees:',
              remoteExecutionFees.toString(),
              'PAS units',
            );
          } catch (error)
        } else {
          console.log('✗ No XCM message found to People Chain');
        }
      } else {
        console.log('✗ Local dry run failed on Polkadot Hub:', dryRunResult.value);
      }

      // 4. TOTAL FEES
      const totalFees = localExecutionFees + deliveryFees + remoteExecutionFees;

      console.log('\n=== Fee Summary (Polkadot Hub → People Chain) ===');
      console.log(
        'Local execution fees:',
        localExecutionFees.toString(),
        'PAS units',
      );
      console.log('Delivery fees:', deliveryFees.toString(), 'PAS units');
      console.log(
        'Remote execution fees:',
        remoteExecutionFees.toString(),
        'PAS units',
      );
      console.log('TOTAL FEES:', totalFees.toString(), 'PAS units');
      console.log(
        'TOTAL FEES:',
        (Number(totalFees) / Number(PAS_UNITS)).toFixed(4),
        'PAS',
      );

      return {
        localExecutionFees,
        deliveryFees,
        remoteExecutionFees,
        totalFees,
      };
    }

    async function main(),
        });

        console.log('\n=== Transaction Details ===');
        console.log('Transaction hex:', (await tx.getEncodedData()).asHex());
        console.log('Ready to submit!');
      } catch (error));
        }
      } finally {
        // Ensure client is always destroyed
        polkadotHubClient.destroy();
      }
    }

    main().catch(console.error);

    ```

## Running the Script

Before running the script, you can use chopsticks to fork the Polkadot Hub TestNet and Paseo People Chain locally. To do so, you can use the following files and commands:

1. Create a new directory called `.chopsticks` and add the files:

    ??? code "paseo-people-chain.yml"

        ```yaml title=".chopsticks/paseo-people-chain.yml"
        endpoint: wss://people-paseo.rpc.amforc.com
        mock-signature-host: true
        block: ${env.PASEO_PEOPLE_CHAIN_BLOCK_NUMBER}
        db: ./db.sqlite

        import-storage:
          Sudo:
            Key: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY # Alice
          System:
            Account:
              -
                -
                  - 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
                - providers: 1
                  data:
                    free: '10000000000000000000'

        ```
    
    ??? code "paseo-asset-hub.yml"

        ```yaml title=".chopsticks/paseo-asset-hub.yml"
        endpoint: wss://asset-hub-paseo-rpc.n.dwellir.com
        mock-signature-host: true
        block: ${env.PASEO_ASSET_HUB_BLOCK_NUMBER}
        db: ./db.sqlite

        import-storage:
          Sudo:
            Key: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY # Alice
          System:
            Account:
              -
                -
                  - 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
                - providers: 1
                  data:
                    free: '10000000000000000000'
        ```

2. Run the following command to fork the Paseo People Chain:

    ```bash
    chopsticks --config=.chopsticks/paseo-people-chain.yml
    ```

    After running the command, you will see the following output:

    <div id="termynal" data-termynal>
      <span data-ty="input"><span class="file-path"></span>chopsticks --config=.chopsticks/paseo-people-chain.yml</span>
      <span data-ty="output">[15:55:22.770] INFO: Paseo People Chain RPC listening on http://[::]:8000 and ws://[::]:8000</span>
      <span data-ty="output">app: "chopsticks"</span>
    </div>

3. Run the following command to fork the Polkadot Hub TestNet chain:

    ```bash
    chopsticks --config=.chopsticks/paseo-asset-hub.yml
    ```

    After running the commands, you will see the following output:

    <div id="termynal" data-termynal>
      <span data-ty="input"><span class="file-path"></span>chopsticks --config=.chopsticks/paseo-asset-hub.yml</span>
      <span data-ty="output">[15:55:22.770] INFO: Paseo Asset Hub Testnet RPC listening on http://[::]:8001 and ws://[::]:8001</span>
      <span data-ty="output">app: "chopsticks"</span>
    </div>
4. Run the script:

    ```bash
    npx ts-node teleport-polkadot-hub-to-people-chain.ts
    ```

After running the script, you will see the following output:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>npx ts-node teleport-polkadot-hub-to-people-chain.ts</span>
  <pre>
=== XCM Teleport: Polkadot Hub TestNet → People Chain ===
From: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5 (Alice on Polkadot Hub TestNet)
To: 14E5nqKAp3oAJcmzgZhUD2RcptBeUBScxKHgJKU4HPNcKVf3 (Beneficiary on People Chain)
Amount: 1 PAS

=== Fee Estimation Process (Polkadot Hub TestNet → People Chain) ===
1. Calculating local execution fees on Polkadot Hub TestNet...
✓ XCM weight (Polkadot Hub TestNet): { ref_time: 1462082000n, proof_size: 19578n }
✓ Local execution fees (Polkadot Hub TestNet): 97890000 PAS units

2. Calculating delivery and remote execution fees...
✓ Local dry run on Polkadot Hub TestNet successful
✓ Found XCM message to People Chain
✓ Delivery fees: 305150000 PAS units

3. Calculating remote execution fees on People Chain
✓ Remote execution fees: 17965000 PAS units

=== Fee Summary (Polkadot Hub TestNet → People Chain) ===
Local execution fees: 97890000 PAS units
Delivery fees: 305150000 PAS units
Remote execution fees: 17965000 PAS units
TOTAL FEES: 421005000 PAS units
TOTAL FEES: 0.0421 PAS

=== Transaction Details ===
Transaction hex: 0x1f03050c00040100000700e40b54023001000002286bee31010100a90f0100000401000002286bee000400010204040d010204000101008eaf04151687736326c9fea17e25fc5287613693c912909cb226aa4794f26a480700bca0650102000400
Ready to submit!

</pre
  >
</div>
## Conclusion

This approach provides accurate fee estimation for XCM teleports from Polkadot Hub TestNet to People Chain by properly simulating execution on both chains and utilizing dedicated runtime APIs for fee calculation. The fee breakdown helps you understand the cost structure of cross-chain operations (asset hub → system parachain) and ensures your transactions have sufficient funds to complete successfully.

The key insight is understanding how asset references change based on the perspective of each chain in the XCM ecosystem, which is crucial for proper fee estimation and XCM construction.


---

Page Title: XCM Tools

- Resolved Markdown: https://docs.polkadot.com/reference/tools/xcm-tools.md
- Canonical (HTML): https://docs.polkadot.com/reference/tools/xcm-tools/
- Summary: Explore essential XCM tools across Polkadot, crafted to enhance cross-chain functionality and integration within the ecosystem.
- Word Count: 836; Token Estimate: 1315
- Last Updated: 2026-04-15T03:51:44+00:00
- Version Hash: sha256:8cb16db434e15dc3d50c509fffaca3363cfecb946ee16176639225315ef8a7a9

# XCM Tools

## Introduction

As described in the [Interoperability](/polkadot-mkdocs/parachains/interoperability/get-started/) section, XCM (Cross-Consensus Messaging) is a protocol used in the Polkadot and Kusama ecosystems to enable communication and interaction between chains. It facilitates cross-chain communication, allowing assets, data, and messages to flow seamlessly across the ecosystem.

As XCM is central to enabling communication between blockchains, developers need robust tools to help interact with, build, and test XCM messages. Several XCM tools simplify working with the protocol by providing libraries, frameworks, and utilities that enhance the development process, ensuring that applications built within the Polkadot ecosystem can efficiently use cross-chain functionalities.

## Popular XCM Tools

### Moonsong Labs XCM Tools

[Moonsong Labs XCM Tools](https://github.com/Moonsong-Labs/xcm-tools) provides a collection of scripts for managing and testing XCM operations between Polkadot SDK-based runtimes. These tools allow performing tasks like asset registration, channel setup, and XCM initialization. Key features include:

- **Asset registration**: Registers assets, setting units per second (up-front fees), and configuring error (revert) codes.
- **XCM initializer**: Initializes XCM, sets default XCM versions, and configures revert codes for XCM-related precompiles.
- **HRMP manipulator**: Manages HRMP channel actions, including opening, accepting, or closing channels.
- **XCM-Transactor-Info-Setter**: Configures transactor information, including extra weight and fee settings.
- **Decode XCM**: Decodes XCM messages on the relay chain or parachains to help interpret cross-chain communication.

To get started, clone the repository and install the required dependencies:

```bash
git clone https://github.com/Moonsong-Labs/xcm-tools && 
cd xcm-tools &&
yarn install
```

For a full overview of each script, visit the [scripts](https://github.com/Moonsong-Labs/xcm-tools/tree/main/scripts) directory or refer to the [official documentation](https://github.com/Moonsong-Labs/xcm-tools/blob/main/README.md) on GitHub.

### ParaSpell

[ParaSpell](/polkadot-mkdocs/reference/tools/paraspell/) is a collection of open-source XCM tools that streamline cross-chain asset transfers and interactions across the Polkadot and Kusama ecosystems. It provides developers with an intuitive interface to build, test, and deploy interoperable dApps, featuring message composition, decoding, and practical utilities for parachain interactions that simplify debugging and cross-chain communication optimization.

### Astar XCM Tools

The [Astar parachain](https://github.com/AstarNetwork/Astar/tree/master) offers a crate with a set of utilities for interacting with the XCM protocol. The [xcm-tools](https://github.com/AstarNetwork/Astar/tree/master/bin/xcm-tools) crate provides a straightforward method for users to locate a sovereign account or calculate an XC20 asset ID. Some commands included by the xcm-tools crate allow users to perform the following tasks:

- **Sovereign accounts**: Obtain the sovereign account address for any parachain, either on the Relay Chain or for sibling parachains, using a simple command.
- **XC20 EVM addresses**: Generate XC20-compatible Ethereum addresses for assets by entering the asset ID, making it easy to integrate assets across Ethereum-compatible environments.
- **Remote accounts**: Retrieve remote account addresses needed for multi-location compatibility, using flexible options to specify account types and parachain IDs.

To start using these tools, clone the [Astar repository](https://github.com/AstarNetwork/Astar) and compile the xcm-tools package:

```bash
git clone https://github.com/AstarNetwork/Astar &&
cd Astar &&
cargo build --release -p xcm-tools
```

After compiling, verify the setup with the following command:

```bash
./target/release/xcm-tools --help
```
For more details on using Astar xcm-tools, consult the [official documentation](https://docs.astar.network/docs/learn/interoperability/xcm/integration/tools/).

### Chopsticks

The Chopsticks library provides XCM functionality for testing XCM messages across networks, enabling you to fork multiple parachains along with a relay chain. For further details, see the [Replay and Dry Run XCMs using Chopsticks](/polkadot-mkdocs/chain-interactions/send-transactions/interoperability/debug-and-preview-xcms/) guide.

### Moonbeam XCM SDK

The [Moonbeam XCM SDK](https://github.com/moonbeam-foundation/xcm-sdk) enables developers to easily transfer assets between chains, either between parachains or between a parachain and the relay chain, within the Polkadot/Kusama ecosystem. With the SDK, you don't need to worry about determining the [Multilocation](https://github.com/polkadot-fellows/xcm-format?tab=readme-ov-file#7-universal-consensus-location-identifiers) of the origin or destination assets or which extrinsics are used on which networks.

The SDK consists of two main packages:

- **[XCM SDK](https://github.com/moonbeam-foundation/xcm-sdk/tree/main/packages/sdk)**: Core SDK for executing XCM transfers between chains in the Polkadot/Kusama ecosystem.
- **[MRL SDK](https://github.com/moonbeam-foundation/xcm-sdk/tree/main/packages/mrl)**: Extension of the XCM SDK for transferring liquidity into and across the Polkadot ecosystem from other ecosystems like Ethereum.

Key features include:

- **Simplified asset transfers**: Abstracts away complex multilocation determinations and extrinsic selection.
- **Cross-ecosystem support**: Enables transfers between Polkadot/Kusama chains and external ecosystems.
- **Developer-friendly API**: Provides intuitive interfaces for cross-chain functionality.
- **Comprehensive documentation**: Includes usage guides and API references for both packages.

For detailed usage examples and API documentation, visit the [official Moonbeam XCM SDK documentation](https://moonbeam-foundation.github.io/xcm-sdk/latest/).
