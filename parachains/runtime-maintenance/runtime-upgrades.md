---
title: Runtime Upgrades
description: Learn how to safely perform runtime upgrades for your Polkadot SDK-based blockchain, including step-by-step instructions.
categories:
- Parachains
url: https://docs.polkadot.com/parachains/runtime-maintenance/runtime-upgrades/
word_count: 1364
token_estimate: 2361
version_hash: sha256:45c1faa840c733e2b917f085a4781c4bf52df4d2001fddab97d440a0da5e99b2
last_updated: '2026-04-15T03:51:44+00:00'
---

# Runtime Upgrades

## Introduction

Upgrading the runtime of your Polkadot SDK-based blockchain is a fundamental feature that allows you to add new functionality, fix bugs, or improve performance without requiring a hard fork. Runtime upgrades are performed by submitting a special extrinsic that replaces the existing on-chain WebAssembly (Wasm) runtime code. This process is trustless, transparent, and can be executed either through governance or using sudo, depending on your chain's configuration.

This tutorial guides you through preparing, submitting, and verifying a runtime upgrade for your parachain or standalone Polkadot SDK-based chain. You add a new dispatchable function to your custom pallet, bump the runtime version, build the new Wasm binary, and submit the upgrade through Polkadot.js Apps.

## Prerequisites

Complete the following before you begin:

- Complete the [Install Polkadot SDK](/polkadot-mkdocs/parachains/install-polkadot-sdk/) guide.
- Set up and run the parachain template by following the [Set Up the Parachain Template](/polkadot-mkdocs/parachains/launch-a-parachain/set-up-the-parachain-template/) tutorial.
- Create and integrate a custom pallet by following the [Create a Custom Pallet](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/create-a-pallet/) tutorial.
- Ensure access to [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer) connected to your local node.

## Start Your Chain

If you don't already have the parachain template running, start the Omni Node in development mode:

```bash
polkadot-omni-node --chain ./chain_spec.json --dev
```

Once the node is running and producing blocks, open [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer?rpc=ws://127.0.0.1:9944) and connect it to your local node at `ws://127.0.0.1:9944`.

Verify your chain is operational. You should see `parachain-template-runtime/1` in the top left corner of the Polkadot.js Apps header, indicating the chain is running with spec version `1`:

![Initial runtime version showing spec version 1 in the Polkadot.js Apps header](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-01.webp)

Keep this chain running in the background.

## Add a New Feature

Extend your existing custom pallet by adding a new dispatchable function to reset the counter to zero. This provides a meaningful upgrade that demonstrates new functionality.

Open your custom pallet's `lib.rs` file and add the following function inside the `#[pallet::call]` block, after the existing dispatchables:

```rust title="custom-pallet/src/lib.rs"
/// Reset the counter to zero.
///
/// The dispatch origin of this call must be _Root_.
///
/// Emits `CounterValueSet` event when successful.
#[pallet::call_index(3)]
#[pallet::weight(0)]
pub fn reset_counter(origin: OriginFor<T>) -> DispatchResult {
	ensure_root(origin)?;
	<CounterValue<T>>::put(0u32);
	Self::deposit_event(Event::CounterValueSet { new_value: 0 });
	Ok(())
}
```

The `reset_counter` function is a Root-only operation that sets the counter value back to zero, regardless of its current state. This is useful for administrative purposes. Unlike the existing `increment` and `decrement` functions that any signed user can call, this reset function requires Root privileges, making it a controlled administrative action.

Ensure that your runtime compiles correctly:

```bash
cargo build --release
```

## Bump the Runtime Version

Before building the final Wasm binary, you must increment the `spec_version` in the runtime. This tells the chain's executor that the new runtime contains changes and should replace the current one.

Open `runtime/src/lib.rs` and find the `VERSION` constant. The current version looks like this:

```rust title="runtime/src/lib.rs"
#[sp_version::runtime_version]
pub const VERSION: RuntimeVersion = RuntimeVersion {
	spec_name: alloc::borrow::Cow::Borrowed("parachain-template-runtime"),
	impl_name: alloc::borrow::Cow::Borrowed("parachain-template-runtime"),
	authoring_version: 1,
	spec_version: 1,
	impl_version: 0,
	apis: apis::RUNTIME_API_VERSIONS,
	transaction_version: 1,
	system_version: 1,
};
```

Change `spec_version` from `1` to `2`:

```rust title="runtime/src/lib.rs" hl_lines="6"
#[sp_version::runtime_version]
pub const VERSION: RuntimeVersion = RuntimeVersion {
	spec_name: alloc::borrow::Cow::Borrowed("parachain-template-runtime"),
	impl_name: alloc::borrow::Cow::Borrowed("parachain-template-runtime"),
	authoring_version: 1,
	spec_version: 2,
	impl_version: 0,
	apis: apis::RUNTIME_API_VERSIONS,
	transaction_version: 1,
	system_version: 1,
};
```

!!! warning
    Forgetting to bump `spec_version` is a common mistake. If you submit a runtime upgrade without incrementing this value, the chain will not recognize it as a new runtime, and the upgrade will have no effect.

## Build the New Runtime

With the new feature added and the version bumped, build the runtime:

```bash
cargo build --release
```

After compilation, verify the Wasm binaries were generated:

<div id="termynal" data-termynal>
  <span data-ty="input"><span class="file-path"></span>ls -la target/release/wbuild/parachain-template-runtime/</span>
  <br />
  <span data-ty>parachain_template_runtime.wasm</span>
  <span data-ty>parachain_template_runtime.compact.wasm</span>
  <span data-ty>parachain_template_runtime.compact.compressed.wasm</span>
</div>
In the next section, you'll use the `.compact.compressed.wasm` file for the upgrade, as it's the smallest and most efficient format.

## Submit the Runtime Upgrade

Now submit the new runtime to the chain using the Sudo pallet through Polkadot.js Apps.

1. Open [Polkadot.js Apps](https://polkadot.js.org/apps/) and connect to your node:

    1. Click on **Developer**.
    2. Select **Extrinsics** from the dropdown.

    ![Navigate to Developer then Extrinsics](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-02.webp)

2. Prepare the sudo call:

    1. Select the **sudo** pallet.
    2. Select the **sudo(call)** extrinsic from the list.

    ![Select the sudo pallet and sudo(call) extrinsic](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-03.webp)

3. In the sudo call:

    1. Select the **system** pallet.
    2. Select the **setCode(code)** extrinsic from the list.

    ![Select system pallet and setCode extrinsic](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-04.webp)

4. Upload the Wasm binary for the `code` parameter:

    1. Click **file upload**.
    2. Select the `parachain_template_runtime.compact.compressed.wasm` file from `target/release/wbuild/parachain-template-runtime/`.

    ![Upload the Wasm file](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-05.webp)

5. Click **Submit Transaction** and sign the transaction with the sudo key.

    ![Sign and submit the transaction](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-06.webp)

!!! info
    On production chains, runtime upgrades are submitted through governance rather than Sudo. The process involves a referendum where token holders vote to approve the upgrade.

After the transaction is included in a block, the runtime upgrade takes effect immediately.

## Verify the Upgrade

### Check Runtime Version

To confirm the upgrade was successful, navigate to the **Developer** dropdown and select **Chain State**. The runtime version in the header changes to `parachain-template-runtime/2`. Additionally, the recent events show `parachainSystem.ValidationFunctionApplied`, confirming the upgrade was applied:

![Explorer showing updated runtime version and upgrade events](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-07.webp)

Query the on-chain runtime version:

1. Query the last runtime upgrade:

    1. Select the **system** pallet.
    2. Select **lastRuntimeUpgrade()** from the query dropdown.

    ![Select system pallet and lastRuntimeUpgrade query](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-08.webp)

2. Click the **+** button to execute the query.

    ![Click the plus button to query](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-09.webp)

3. The result should show `specVersion: 2`, confirming the upgrade was applied.

    ![Verify specVersion is 2](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-10.webp)

### Test New Functionality

Navigate to **Developer** > **Extrinsics** and select your **customPallet**. The new `resetCounter` function appears alongside the existing `increment`, `decrement`, and `setCounterValue` functions.

![Custom pallet showing the new resetCounter function](/polkadot-mkdocs/images/parachains/runtime-maintenance/runtime-upgrades/runtime-upgrade-11.webp)

Test the new functionality:

1. Increment the counter using the existing `increment` function.
2. Use the new `resetCounter` function (requires sudo/root privileges).
3. Verify the counter value is reset to `0`.

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Storage Migrations__

    ---

    Learn how to write and manage storage migrations when runtime upgrades change how data is stored on-chain.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/runtime-maintenance/storage-migrations/)

-   <span class="badge tutorial">Tutorial</span> __Create a Custom Pallet__

    ---

    Learn how to build a custom pallet from scratch to add new functionality to your Polkadot SDK-based runtime.

    [:octicons-arrow-right-24: Get Started](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/create-a-pallet/)

</div>
