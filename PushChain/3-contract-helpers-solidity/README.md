You’ve already:

- Deployed a simple Solidity contract to Push Chain Donut Testnet.
- Sent a universal transaction from Sepolia to a `SimpleCounter` contract that was **already deployed** on Push Chain (its address was given to you in the previous chapter).

In this chapter you switch perspective: instead of just sending cross-chain calls, you’ll learn **how a Solidity contract on Push Chain can understand who the user really is** by talking to a core system contract called `UEAFactory`.

We provide a minimal Solidity contract `UEAFactoryDemo.sol` that demonstrates how to call two essential Push Chain `UEAFactory` functions from on-chain code:

- **`getOriginForUEA()`** – Given a Push Chain address (usually a UEA), determines whether it’s a native Push Chain account or a Universal Executor Account (UEA) representing an external-chain user, and returns that user’s **Universal Origin Account** identity.
- **`getUEAForOrigin()`** – Given an origin identity, computes the **deterministic UEA address** for that user on Push Chain and tells you whether it’s already deployed.

> **Goal of this chapter**
>
> Teach you how to **discover a user’s origin chain and wallet** and how to **compute / look up their UEA address** from inside your Solidity contracts on Push Chain.

## Why Universal Account Discovery matters

From your contract’s point of view, a user on Push Chain is often just `msg.sender` – but behind that address there may be a **user coming from another chain**. Universal Account Discovery lets you bridge that gap.

Developers typically use `getOriginForUEA()` and `getUEAForOrigin()` to:

- **Personalize logic based on origin chain**: e.g. apply different rules or fee discounts for users coming from Solana vs Ethereum.
- **Enforce per-user or per-origin limits**: rate-limit or cap usage using the stable origin identity rather than the transient UEA address.
- **Implement allowlists / compliance rules**: only allow calls from specific origin chains or from a known set of origin accounts.
- **Link analytics back to the home chain**: attribute activity on Push Chain to the user’s original wallet and chain for reporting.

This contract does **not** move funds or change state; it’s a **read-only helper** you can call from dApps or other contracts to understand who you’re dealing with.

## What the contract does

The `UEAFactoryDemo.sol` contract is a **normal Solidity contract** that you can deploy to Push Chain (for example, using Remix as in chapter 1). It only **reads data** from a predeployed system contract:

- The `UEAFactory` system contract lives at `0x00000000000000000000000000000000000000eA` on Push Chain.
- It is **already deployed for you**; you never deploy or upgrade it yourself.
- `UEAFactoryDemo` just **interacts** with `UEAFactory` – it does not deploy any other contracts.

Once deployed, `UEAFactoryDemo` exposes two view functions:

- **`discoverOrigin()`** – Calls `getOriginForUEA(msg.sender)` on `UEAFactory` and returns:
  - a `UniversalAccountId` describing the caller’s origin (chain namespace, chain ID, and original wallet bytes), and
  - a boolean `isUEA` that tells you whether the caller (`msg.sender`) is a Universal Executor Account (UEA).
- **`discoverUEAForOrigin(UniversalAccountId account)`** – Calls `getUEAForOrigin(account)` on `UEAFactory` and returns:
  - the deterministic `uea` address for that origin on Push Chain, and
  - an `isDeployed` flag indicating whether that UEA contract has actually been created yet.

## How to try this in Remix

If you followed chapter 1, you already know how to deploy a Solidity contract to Push Chain Donut Testnet. Here’s the short version for `UEAFactoryDemo.sol`:

1. Open <a href="https://remix.ethereum.org" target="_blank">Remix IDE</a> and create a new file named `UEAFactoryDemo.sol`.
2. Paste the `UEAFactoryDemo.sol` contract from this folder.
3. In MetaMask, switch to **Push Chain Donut Testnet** (same network you used before).
4. In Remix, select **Injected Provider – MetaMask** and deploy `UEAFactoryDemo`.
5. In the **Deployed Contracts** section, expand your `UEAFactoryDemo` instance and call:
   - `discoverOrigin()` – you’ll get back the origin identity of `msg.sender` plus `isUEA`.
   - Optionally, pass that origin into `discoverUEAForOrigin(...)` to see the deterministic UEA address and whether it’s deployed.

> In the previous chapter, the `SimpleCounter` contract address was given to you because it was **already deployed**. Here, you deploy `UEAFactoryDemo` yourself, but you still rely on the **predeployed** `UEAFactory` at `0x00000000000000000000000000000000000000eA`.

## Notes

- The `UEAFactory` address is the predeployed contract used by Push Chain.
- If you want to compare with a more feature-rich example, check the <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/tutorials/tutorial-universal-counter/" target="_blank">Universal Counter tutorial</a> which includes a React app with an interface for interacting with the contract.
- `getUEAForOrigin` can return a deterministic UEA address even if the account is not yet deployed (`isDeployed == false`).

## Sample responses (what the return values mean)

- `discoverOrigin()` returns:

```json
[["eip155","11155111","0xfd6c2fe69be13d8be379ccb6c9306e74193ec1a9"], true]
```

This means:

- The caller’s origin lives on an EVM chain (`"eip155"`) with chain ID `11155111` (Ethereum Sepolia).
- The original wallet on that chain is `0xfd6c2fe69be13d8be379ccb6c9306e74193ec1a9`.
- The final `true` indicates that `msg.sender` on Push Chain is a **UEA representing that origin wallet**, not a native Push Chain EOA.

- `discoverUEAForOrigin(UniversalAccountId)` returns:

```json
["0x3445AEE60c70c9f5A947A28B879ca6B449B0a4ce", false]
```

This means:

- `0x3445AEE60c70c9f5A947A28B879ca6B449B0a4ce` is the **deterministic UEA address** for that origin on Push Chain.
- `false` indicates that the UEA contract has **not yet been deployed** (for example, the user has not executed a transaction that required creating it).

## References

- Tutorial example: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/tutorials/tutorial-universal-counter/" target="_blank">Universal Counter (contract calling getOriginForUEA)</a>
- Concepts: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/important-concepts/#account-types-on-push-chain" target="_blank">Account types on Push Chain</a>
- Contract Helpers: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/contract-helpers/#ueafactory--getoriginforuea" target="_blank">UEAFactory → getOriginForUEA</a>
- Contract Helpers: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/contract-helpers/#ueafactory--getueafororigin" target="_blank">UEAFactory → getUEAForOrigin</a>
- Next steps: see the <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/tutorials/tutorial-universal-counter/" target="_blank">Universal Counter tutorial</a> for a complete example that uses these helpers in a more feature-rich Solidity contract and React app.

