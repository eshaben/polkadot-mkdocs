---
title: Unit Test Pallets
description: Learn how to efficiently test pallets in the Polkadot SDK, ensuring the reliability and security of your pallets operations.
categories:
- Parachains
url: https://docs.polkadot.com/parachains/customize-runtime/pallet-development/pallet-testing/
word_count: 1759
token_estimate: 3672
version_hash: sha256:dbec3874bb8c0f2187398b7d0db09c4c0f96ad3fc26ff969ea6ad73e5cee737f
last_updated: '2026-04-15T03:51:44+00:00'
---

# Unit Test Pallets

## Introduction

Unit testing in the Polkadot SDK helps ensure that the functions provided by a pallet behave as expected. It also confirms that data and events associated with a pallet are processed correctly during interactions. With your mock runtime in place from the [previous guide](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/mock-runtime/), you can now write comprehensive tests that verify your pallet's behavior in isolation.

In this guide, you'll learn how to:

- Structure test modules effectively.
- Test dispatchable functions.
- Verify storage changes.
- Check event emission.
- Test error conditions.
- Use genesis configurations in tests.

## Prerequisites

Before you begin, ensure you:

- Completed the [Make a Custom Pallet](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/create-a-pallet/) guide.
- Completed the [Mock Your Runtime](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/mock-runtime/) guide.
- Configured custom counter pallet with mock runtime in `pallets/pallet-custom`.
- Understood the basics of [Rust testing](https://doc.rust-lang.org/book/ch11-00-testing.html).

## Understanding FRAME Testing Tools

[FRAME (Framework for Runtime Aggregation of Modularized Entities)](/polkadot-mkdocs/reference/glossary/#frame-framework-for-runtime-aggregation-of-modularized-entities) provides specialized testing macros and utilities that make pallet testing more efficient.

### Assertion Macros

- **[`assert_ok!`](https://paritytech.github.io/polkadot-sdk/master/frame_support/macro.assert_ok.html)** - Asserts that a dispatchable call succeeds.
- **[`assert_noop!`](https://paritytech.github.io/polkadot-sdk/master/frame_support/macro.assert_noop.html)** - Asserts that a call fails without changing state (no operation).
- **[`assert_eq!`](https://doc.rust-lang.org/std/macro.assert_eq.html)** - Standard Rust equality assertion.

!!!info "`assert_noop!` Explained"
    Use `assert_noop!` to ensure the operation fails without any state changes. This is critical for testing error conditions - it verifies both that the error occurs AND that no storage was modified.

### System Pallet Test Helpers

The [`frame_system`](https://paritytech.github.io/polkadot-sdk/master/frame_system/index.html) pallet provides useful methods for testing:

- **[`System::events()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.events)** - Returns all events emitted during the test.
- **[`System::assert_last_event()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.assert_last_event)** - Asserts the last event matches expectations.
- **[`System::set_block_number()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/pallet/struct.Pallet.html#method.set_block_number)** - Sets the current block number.

!!!info "Events and Block Number"
    Events are not emitted on block 0 (genesis block). If you need to test events, ensure you set the block number to at least 1 using `System::set_block_number(1)`.

### Origin Types

- **[`RuntimeOrigin::root()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/enum.RawOrigin.html#variant.Root)** - Root/sudo origin for privileged operations.
- **[`RuntimeOrigin::signed(account)`](https://paritytech.github.io/polkadot-sdk/master/frame_system/enum.RawOrigin.html#variant.Signed)** - Signed origin from a specific account.
- **[`RuntimeOrigin::none()`](https://paritytech.github.io/polkadot-sdk/master/frame_system/enum.RawOrigin.html#variant.None)** - No origin (typically fails for most operations).

Learn more about origins in the [FRAME Origin reference document](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/frame_origin/index.html).

## Create the Tests Module

Create a new file for your tests within the pallet directory:

1. Navigate to your pallet directory:

    ```bash
    cd pallets/pallet-custom/src
    ```

2. Create a new file named `tests.rs`:

    ```bash
    touch tests.rs
    ```

3. Open `src/lib.rs` and add the tests module declaration after the mock module:

    ```rust title="src/lib.rs"
    #![cfg_attr(not(feature = "std"), no_std)]

    pub use pallet::*;

    #[cfg(test)]
    mod mock;

    #[cfg(test)]
    mod tests;

    #[frame::pallet]
    pub mod pallet {
        // ... existing pallet code
    }
    ```

## Set Up the Test Module

Open `src/tests.rs` and add the basic structure with necessary imports:

```rust
use crate::{mock::*, Error, Event};
use frame::deps::frame_support::{assert_noop, assert_ok};
use frame::deps::sp_runtime::DispatchError;
```

This setup imports:

- The mock runtime and test utilities from `mock.rs`
- Your pallet's `Error` and `Event` types
- FRAME's assertion macros via `frame::deps`
- `DispatchError` for testing origin checks

???+ code "Complete Pallet Code Reference"
    Here's the complete pallet code that you'll be testing throughout this guide:

    ```rust
    #![cfg_attr(not(feature = "std"), no_std)]

    extern crate alloc;

    pub use pallet::*;

    #[frame::pallet]
    pub mod pallet {
        use alloc::vec::Vec;
        use frame::prelude::*;

        #[pallet::pallet]
        pub struct Pallet<T>(_);

        #[pallet::config]
        pub trait Config: frame_system::Config {
            type RuntimeEvent: From<Event<Self>> + IsType<<Self as frame_system::Config>::RuntimeEvent>;

            #[pallet::constant]
            type CounterMaxValue: Get<u32>;
        }

        #[pallet::event]
        #[pallet::generate_deposit(pub(super) fn deposit_event)]
        pub enum Event<T: Config> {
            CounterValueSet {
                new_value: u32,
            },
            CounterIncremented {
                new_value: u32,
                who: T::AccountId,
                amount: u32,
            },
            CounterDecremented {
                new_value: u32,
                who: T::AccountId,
                amount: u32,
            },
        }

        #[pallet::error]
        pub enum Error<T> {
            NoneValue,
            Overflow,
            Underflow,
            CounterMaxValueExceeded,
        }

        #[pallet::storage]
        pub type CounterValue<T> = StorageValue<_, u32, ValueQuery>;

        #[pallet::storage]
        pub type UserInteractions<T: Config> = StorageMap<
            _,
            Blake2_128Concat,
            T::AccountId,
            u32,
            ValueQuery
        >;

        #[pallet::genesis_config]
        #[derive(DefaultNoBound)]
        pub struct GenesisConfig<T: Config> {
            pub initial_counter_value: u32,
            pub initial_user_interactions: Vec<(T::AccountId, u32)>,
        }

        #[pallet::genesis_build]
        impl<T: Config> BuildGenesisConfig for GenesisConfig<T> {
            fn build(&self)
            }
        }

        #[pallet::call]
        impl<T: Config> Pallet<T> {
            #[pallet::call_index(0)]
            #[pallet::weight(0)]
            pub fn set_counter_value(origin: OriginFor<T>, new_value: u32) -> DispatchResult {
                ensure_root(origin)?;
                ensure!(new_value <= T::CounterMaxValue::get(), Error::<T>::CounterMaxValueExceeded);
                CounterValue::<T>::put(new_value);
                Self::deposit_event(Event::CounterValueSet { new_value });
                Ok(())
            }

            #[pallet::call_index(1)]
            #[pallet::weight(0)]
            pub fn increment(origin: OriginFor<T>, amount: u32) -> DispatchResult {
                let who = ensure_signed(origin)?;
                let current_value = CounterValue::<T>::get();
                let new_value = current_value.checked_add(amount).ok_or(Error::<T>::Overflow)?;
                ensure!(new_value <= T::CounterMaxValue::get(), Error::<T>::CounterMaxValueExceeded);
                CounterValue::<T>::put(new_value);
                UserInteractions::<T>::mutate(&who, |count| {
                    *count = count.saturating_add(1);
                });
                Self::deposit_event(Event::CounterIncremented { new_value, who, amount });
                Ok(())
            }

            #[pallet::call_index(2)]
            #[pallet::weight(0)]
            pub fn decrement(origin: OriginFor<T>, amount: u32) -> DispatchResult {
                let who = ensure_signed(origin)?;
                let current_value = CounterValue::<T>::get();
                let new_value = current_value.checked_sub(amount).ok_or(Error::<T>::Underflow)?;
                CounterValue::<T>::put(new_value);
                UserInteractions::<T>::mutate(&who, |count| {
                    *count = count.saturating_add(1);
                });
                Self::deposit_event(Event::CounterDecremented { new_value, who, amount });
                Ok(())
            }
        }
    }
    ```

## Write Your First Test

Let's start with a simple test to verify the increment function works correctly.

### Test Basic Increment

Test that the increment function increases counter value and emits events.

```rust
#[test]
fn increment_works()
            .into(),
        );

        // Check user interactions were tracked
        assert_eq!(crate::UserInteractions::<Test>::get(account), 1);
    });
}
```

Run your first test:

```bash
cargo test --package pallet-custom increment_works
```

You should see:

```
running 1 test
test tests::increment_works ... ok
```

Congratulations! You've written and run your first pallet test.

## Test Error Conditions

Now let's test that our pallet correctly handles errors. Error testing is crucial to ensure your pallet fails safely.

### Test Overflow Protection

Test that incrementing at u32::MAX fails with Overflow error.

```rust
#[test]
fn increment_fails_on_overflow());
}
```

Test overflow protection:

```bash
cargo test --package pallet-custom increment_fails_on_overflow
```

### Test Underflow Protection

Test that decrementing below zero fails with Underflow error.

```rust
#[test]
fn decrement_fails_on_underflow());
}
```

Verify underflow protection:

```bash
cargo test --package pallet-custom decrement_fails_on_underflow
```

## Test Access Control

Verify that origin checks work correctly and unauthorized access is prevented.

### Test Root-Only Access

Test that set_counter_value requires root origin and rejects signed origins.

```rust
#[test]
fn set_counter_value_requires_root());
}
```

Test access control:

```bash
cargo test --package pallet-custom set_counter_value_requires_root
```

## Test Event Emission

Verify that events are emitted correctly with the right data.

### Test Event Data

The [`increment_works`](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/pallet-testing/#test-basic-increment) test (shown earlier) already demonstrates event testing by:

1. Setting the block number to 1 to enable event emission.
2. Calling the dispatchable function.
3. Using `System::assert_last_event()` to verify the correct event was emitted with expected data.

This pattern applies to all dispatchables that emit events. For a dedicated event-only test focusing on the `set_counter_value` function:

Test that set_counter_value updates storage and emits correct event.

```rust
#[test]
fn set_counter_value_works().into());
    });
}
```

Run the event test:

```bash
cargo test --package pallet-custom set_counter_value_works
```

## Test Genesis Configuration

Verify that genesis configuration works correctly.

### Test Genesis Setup

Test that genesis configuration correctly initializes counter and user interactions.

```rust
#[test]
fn genesis_config_works());
}
```

Test genesis configuration:

```bash
cargo test --package pallet-custom genesis_config_works
```

## Run All Tests

Now run all your tests together:

```bash
cargo test --package pallet-custom
```

You should see all tests passing:

<div id="termynal" data-termynal>
  <span data-ty="input">$ cargo test --package pallet-custom</span>
  <span data-ty>running 15 tests</span>
  <span data-ty>test mock::__construct_runtime_integrity_test::runtime_integrity_tests ... ok</span>
  <span data-ty>test mock::test_genesis_config_builds ... ok</span>
  <span data-ty>test tests::decrement_fails_on_underflow ... ok</span>
  <span data-ty>test tests::decrement_tracks_multiple_interactions ... ok</span>
  <span data-ty>test tests::decrement_works ... ok</span>
  <span data-ty>test tests::different_users_tracked_separately ... ok</span>
  <span data-ty>test tests::genesis_config_works ... ok</span>
  <span data-ty>test tests::increment_fails_on_overflow ... ok</span>
  <span data-ty>test tests::increment_respects_max_value ... ok</span>
  <span data-ty>test tests::increment_tracks_multiple_interactions ... ok</span>
  <span data-ty>test tests::increment_works ... ok</span>
  <span data-ty>test tests::mixed_increment_and_decrement_works ... ok</span>
  <span data-ty>test tests::set_counter_value_requires_root ... ok</span>
  <span data-ty>test tests::set_counter_value_respects_max_value ... ok</span>
  <span data-ty>test tests::set_counter_value_works ... ok</span>
  <span data-ty></span>
  <span data-ty>test result: ok. 15 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out</span>
</div>

!!!note "Mock Runtime Tests"
    You'll notice 2 additional tests from the `mock` module:

    - `mock::__construct_runtime_integrity_test::runtime_integrity_tests` - Auto-generated test that validates runtime construction
    - `mock::test_genesis_config_builds` - Validates that genesis configuration builds correctly

    These tests are automatically generated from your mock runtime setup and help ensure the test environment itself is valid.

Congratulations! You have a well-tested pallet covering the essential testing patterns!

These tests demonstrate comprehensive coverage including basic operations, error conditions, access control, event emission, state management, and genesis configuration. As you build more complex pallets, you'll apply these same patterns to test additional functionality.

??? code "Full Test Suite Code"
    Here's the complete `tests.rs` file for quick reference:

    ```rust
    use crate::{mock::*, Error, Event};
    use frame::deps::frame_support::{assert_noop, assert_ok};
    use frame::deps::sp_runtime::DispatchError;

    #[test]
    fn set_counter_value_works().into());
        });
    }

    #[test]
    fn set_counter_value_requires_root());
    }

    #[test]
    fn set_counter_value_respects_max_value());
    }

    #[test]
    fn increment_works()
                .into(),
            );

            // Check user interactions were tracked
            assert_eq!(crate::UserInteractions::<Test>::get(account), 1);
        });
    }

    #[test]
    fn increment_tracks_multiple_interactions());
    }

    #[test]
    fn increment_fails_on_overflow());
    }

    #[test]
    fn increment_respects_max_value());
    }

    #[test]
    fn decrement_works()
                .into(),
            );

            // Check user interactions were tracked
            assert_eq!(crate::UserInteractions::<Test>::get(account), 1);
        });
    }

    #[test]
    fn decrement_fails_on_underflow());
    }

    #[test]
    fn decrement_tracks_multiple_interactions());
    }

    #[test]
    fn mixed_increment_and_decrement_works());
    }

    #[test]
    fn different_users_tracked_separately());
    }

    #[test]
    fn genesis_config_works());
    }
    ```

## Where to Go Next

<div class="grid cards" markdown>

-   <span class="badge guide">Guide</span> __Benchmark Your Pallet__

    ---

    Learn how to benchmark extrinsics in your custom pallet to generate precise weight calculations suitable for production use.

    [:octicons-arrow-right-24: Integrate](/polkadot-mkdocs/parachains/customize-runtime/pallet-development/benchmark-pallet/)

</div>
