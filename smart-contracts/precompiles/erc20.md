---
title: Interact with the ERC20 Precompile
description: Learn how to use the ERC20 precompile to interact with assets from the assets pallet using standard ERC20 token interfaces in your smart contracts.
categories:
- Smart Contracts
url: https://docs.polkadot.com/smart-contracts/precompiles/erc20/
word_count: 1757
token_estimate: 3370
version_hash: sha256:f7a3a76009c6364df5316066bde05f7e895abfb4cae37786cfd9d1168d4570ad
last_updated: '2026-04-15T03:51:44+00:00'
---

# ERC20 Precompile

<div class="erc20-asset-converter-container">
  <h2 class="erc20-asset-converter-heading">ERC20 Precompile Address Converter</h2>
  <p class="erc20-asset-converter-subtitle">Convert asset IDs and foreign asset indexes to ERC20 precompile addresses</p>
  <div class="erc20-asset-converter-box">
    <div class="erc20-asset-converter-type-selector">
      <label class="erc20-asset-converter-type-option">
        <input type="radio" name="erc20AssetType" class="erc20-asset-converter-type-radio" value="trustBacked" checked> Trust-Backed Asset
      </label>
      <label class="erc20-asset-converter-type-option">
        <input type="radio" name="erc20AssetType" class="erc20-asset-converter-type-radio" value="foreign"> Foreign Asset
      </label>
      <label class="erc20-asset-converter-type-option">
        <input type="radio" name="erc20AssetType" class="erc20-asset-converter-type-radio" value="pool"> Pool Asset
      </label>
    </div>
    <div class="erc20-asset-converter-input-section">
      <div class="erc20-asset-converter-input-wrapper">
        <label class="erc20-asset-converter-label" id="erc20AssetIdLabel" for="erc20AssetId">Asset ID (Decimal)</label>
        <input type="number" id="erc20AssetId" class="erc20-asset-converter-input" min="0" max="4294967295" placeholder="1984" inputmode="numeric">
      </div>
      <div id="erc20AssetIdError" class="erc20-asset-converter-error"></div>
    </div>
    <div id="erc20Results" class="erc20-asset-converter-results hidden">
      <div class="erc20-asset-converter-result-item">
        <span class="erc20-asset-converter-result-label" id="erc20ResultLabel">ERC20 Precompile Address</span>
        <div class="erc20-asset-converter-output-wrapper">
          <input type="text" id="erc20AddressOutput" class="erc20-asset-converter-output" readonly spellcheck="false">
          <button type="button" class="erc20-asset-converter-copy-button" title="Copy to clipboard" aria-label="Copy to clipboard">
            <svg class="erc20-asset-converter-copy-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect x="9" y="9" width="13" height="13" rx="2" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/><path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
            <svg class="erc20-asset-converter-check-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M20 6L9 17L4 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
            <span class="erc20-asset-converter-copy-feedback" aria-live="polite">Copied!</span>
          </button>
        </div>
      </div>
    </div>
    <div id="erc20FormatInfo" class="erc20-asset-converter-info"></div>
  </div>
</div>
## Introduction

The ERC20 precompile provides a standard ERC20 token interface for interacting with assets managed by the [Assets pallet](https://paritytech.github.io/polkadot-sdk/master/pallet_assets/index.html), helping smart contracts to interact with native Polkadot assets (such as USDT, USDC, and other tokens) using familiar Ethereum-style ERC20 calls. Polkadot Hub runs three instances of the Assets pallet — Trust-Backed Assets, Foreign Assets, and Pool Assets — each mapped to a distinct ERC20 precompile address suffix.

Each asset is mapped to a unique precompile address based on its asset ID or foreign asset index. The precompile implements core ERC20 functionality:

- **Token transfers**: Send assets between accounts using standard `transfer` and `transferFrom` methods.
- **Approvals and allowances**: Manage spending permissions with `approve` and `allowance`.
- **Balance queries**: Check token balances with `balanceOf` and total supply with `totalSupply`.

## Supported Asset Types

The ERC20 precompile supports three categories of assets, each with its own address suffix.

### Trust-Backed Assets

Trust-Backed Assets are created directly in the Assets pallet on Polkadot Hub and assigned a u32 asset ID.

- **Address suffix**: `01200000`
- **Address format**: `0x` + assetId (8 hex digits, zero-padded) + 24 zero digits + `01200000`
- **Example**: Asset ID `1984` → `0x000007C000000000000000000000000001200000`

### Foreign Assets

Foreign Assets originate from other chains and are identified on-chain by their XCM Location. The ERC20 precompile uses a u32 index—not the XCM Location directly—to derive the precompile address.

- **Address suffix**: `02200000`
- **Address format**: `0x` + foreignAssetIndex (8 hex digits, zero-padded) + 24 zero digits + `02200000`
- **Example**: Foreign Asset Index `0` → `0x0000000000000000000000000000000002200000`

#### Deriving the Foreign Asset Index

Since foreign asset IDs are XCM Locations (not simple integers), the runtime assigns each foreign asset a sequential u32 index when it is registered. To derive the ERC20 precompile address for a foreign asset, follow these steps:

1. Get the XCM location of the foreign asset (for example, `{ parents: 1, interior: X1(Parachain(2313)) }`).
2. Query the index in [Polkadot.js Apps](https://polkadot.js.org/apps/):

    1. Navigate to **Developer > Chain State**.
    2. Select **assetsPrecompiles** from the module dropdown.
    3. Select **foreignAssetIdToAssetIndex** from the call dropdown.
    4. Pass the XCM Location and note the returned u32 value (for example, `5`).

    ![](/polkadot-mkdocs/images/smart-contracts/precompiles/erc20/erc20-01.webp)

3. Enter that u32 index into the Foreign Asset mode of the converter above to derive the ERC20 precompile address.

### Pool Assets

Pool Assets are created by liquidity pool operations on Polkadot Hub and assigned a u32 asset ID.

- **Address suffix**: `03200000`
- **Address format**: `0x` + assetId (8 hex digits, zero-padded) + 24 zero digits + `03200000`
- **Example**: Pool Asset ID `0` → `0x0000000000000000000000000000000003200000`

## Precompile Interface

The ERC20 precompile implements a subset of the standard ERC20 interface. The following functions are available:

```solidity title="IERC20-precompile.sol"
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    // Implemented functions
    function totalSupply() external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}
```

!!!warning "Metadata Functions Not Available"
    The optional ERC20 metadata functions (`name()`, `symbol()`, `decimals()`) are **not implemented** in this precompile. These functions are only available through the Assets pallet's storage, not via the ERC20 interface.

## Query Functions

### Get Total Supply

Returns the total number of tokens in circulation for this asset.

```solidity
function totalSupply() external view returns (uint256);
```

**Returns:**

- **`uint256`**: The total supply of tokens

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
uint256 supply = token.totalSupply();
```

### Get Balance

Returns the token balance of a specific account address.

```solidity
function balanceOf(address account) external view returns (uint256);
```

**Parameters:**

- **`account`**: The address to query

**Returns:**

- **`uint256`**: The token balance of the account

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
address user = 0x1234567890123456789012345678901234567890;
uint256 balance = token.balanceOf(user);
```

### Check Allowance

Returns the amount of tokens that the spender is allowed to spend on behalf of the owner.

```solidity
function allowance(address owner, address spender) external view returns (uint256);
```

**Parameters:**

- **`owner`**: The account that owns the tokens
- **`spender`**: The account authorized to spend

**Returns:**

- **`uint256`**: The remaining allowance

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
address owner = 0x1111111111111111111111111111111111111111;
address spender = 0x2222222222222222222222222222222222222222;
uint256 remaining = token.allowance(owner, spender);
```

## Token Operations

### Transfer Tokens

Transfers tokens from the caller's account to the recipient address.

```solidity
function transfer(address to, uint256 amount) external returns (bool);
```

**Parameters:**

- **`to`**: The recipient address
- **`amount`**: The amount of tokens to transfer

**Returns:**

- **`bool`**: `true` if the transfer was successful

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
address recipient = 0x3333333333333333333333333333333333333333;
uint256 amount = 1000 * 10**10; // Assuming 10 decimals

bool success = token.transfer(recipient, amount);
require(success, "Transfer failed");
```

!!!warning
    The transfer will fail if the caller doesn't have sufficient balance.

### Approve Spending

Approves the spender to withdraw up to a specified amount from the caller's account.

```solidity
function approve(address spender, uint256 amount) external returns (bool);
```

**Parameters:**

- **`spender`**: The address authorized to spend tokens
- **`amount`**: The maximum amount the spender can withdraw

**Returns:**

- **`bool`**: `true` if the approval was successful

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
address spender = 0x4444444444444444444444444444444444444444;
uint256 amount = 500 * 10**10;

bool success = token.approve(spender, amount);
require(success, "Approval failed");
```

### Transfer From

Transfers tokens from one account to another using the allowance mechanism. The caller must have sufficient allowance from the `from` account.

```solidity
function transferFrom(address from, address to, uint256 amount) external returns (bool);
```

**Parameters:**

- **`from`**: The account to transfer from
- **`to`**: The recipient address
- **`amount`**: The amount of tokens to transfer

**Returns:**

- **`bool`**: `true` if the transfer was successful

**Example usage:**

```solidity
IERC20 token = IERC20(0x000007C000000000000000000000000001200000);
address owner = 0x5555555555555555555555555555555555555555;
address recipient = 0x6666666666666666666666666666666666666666;
uint256 amount = 250 * 10**10;

bool success = token.transferFrom(owner, recipient, amount);
require(success, "Transfer from failed");
```

For the complete implementation, refer to the [ERC20 precompile source code](https://github.com/paritytech/polkadot-sdk/blob/11be995be95ac1e25a5b2a6dd941006e7097bffc/substrate/frame/assets/precompiles/src/lib.rs) in the Polkadot SDK.

## Common Trust-Backed Asset IDs

The following well-known Trust-Backed Assets are registered on Polkadot Hub and accessible via the ERC20 precompile:

| Asset ID | Symbol | Name | Decimals | ERC20 Precompile Address |
|:---:|:---:|:---:|:---:|:---:|
| 1984 | USDt | Tether USD | 6 | `0x000007C000000000000000000000000001200000` |
| 1337 | USDC | USD Coin | 6 | `0x0000053900000000000000000000000001200000` |

!!! note
    The on-chain symbol for Tether on Polkadot Hub is `USDt`, which is commonly referred to as "USDT" on exchanges and in wallets.

## Interact with the ERC20 Precompile

To interact with the ERC20 precompile in [Remix IDE](/polkadot-mkdocs/smart-contracts/dev-environments/remix/):

1. Create a new file called `IERC20-precompile.sol` in Remix
2. Copy and paste the `IERC20` interface code shown above into the file

    ![](/polkadot-mkdocs/images/smart-contracts/precompiles/erc20/erc20-02.webp)

3. Compile the interface by selecting the compile button or using **Ctrl + S**
4. Calculate the ERC20 precompile address for your asset using the converter above. Select the appropriate asset type (Trust-Backed, Foreign, or Pool), then enter the asset ID or foreign asset index.

    - **Trust-Backed example**: Asset ID `1984` (USDt) → `0x000007C000000000000000000000000001200000`
    - **Foreign Asset example**: Foreign Asset Index `0` → `0x0000000000000000000000000000000002200000`

5. In the **Deploy & Run Transactions** tab, select the `IERC20` interface from the contract dropdown
6. Enter the calculated precompile address in the **At Address** input field
7. Select the **At Address** button to connect to the precompile

    ![](/polkadot-mkdocs/images/smart-contracts/precompiles/erc20/erc20-03.webp)

Once connected, you can interact with any of the ERC20 precompile functions directly through the Remix interface.

![](/polkadot-mkdocs/images/smart-contracts/precompiles/erc20/erc20-04.webp)

## Conclusion

The ERC20 precompile provides seamless integration between Polkadot's native asset management and Ethereum's familiar token standard. By mapping asset IDs to deterministic precompile addresses, developers can interact with native Polkadot assets using standard ERC20 interfaces.

Whether you're building DeFi protocols, token swaps, or any application requiring asset interactions, the ERC20 precompile enables you to leverage Polkadot's rich asset ecosystem with the same tools and patterns used in Ethereum development.

## Reference

- [ERC20 precompile source code](https://github.com/paritytech/polkadot-sdk/blob/11be995be95ac1e25a5b2a6dd941006e7097bffc/substrate/frame/assets/precompiles/src/lib.rs)
- [Assets pallet documentation](https://paritytech.github.io/polkadot-sdk/master/pallet_assets/index.html)
- [EIP-20: Token Standard](https://eips.ethereum.org/EIPS/eip-20)
