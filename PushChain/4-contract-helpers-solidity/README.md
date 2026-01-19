## Universal Account Discovery (Solidity)

You’ve already:

- Deployed a Solidity contract to Push Chain Donut Testnet.
- Sent a universal transaction to a `SimpleCounter` contract on Push Chain.

In this chapter you’ll learn how a Push Chain contract can answer a key question:

> **Who is the user behind `msg.sender`?**

This is an imperative question because Push Chain allows universal smart contracts. It means users from any chain ( *not just Push Chain* ) can call the contract.

Therefore, on Push Chain, `msg.sender` might be:

- a native Push Chain account, or
- a **UEA** (Universal Executor Account) representing a user from another chain.

## Goal

In this chapter, we will mainly learn how to do **universal account discovery inside Solidity**:

1. **Get Origin for UEA**: Find the caller’s **origin identity** (chain + wallet). 
- This means for a given address, we can determine if the address is a Push Chain address or a UEA ( external chain user from ethereum, solana or base etc).
- To get this, we use the already available function `getOriginForUEA()`.

2. **Get UEA for Origin**: Compute the caller’s **deterministic UEA** 
- This means for a given origin wallet, we can compute the deterministic UEA address for that wallet on push Chain. We can also check whether this UEA is already deployed.
- To get this, we use the already available function `getUEAForOrigin()`.

*For example*:
- Bob on Ethereum has wallet `0xABC...`.
- We can compute Bob’s deterministic UEA address on Push Chain with `getUEAForOrigin(...)` (and see if it’s deployed).


## Why it matters

You use origin discovery to:

- Apply different rules per origin chain (Ethereum vs Solana, etc.)
- Rate-limit per real user (origin) instead of per executor address (UEA)
- Build allowlists / compliance rules based on origin chain + wallet
- Attribute analytics back to the user’s home chain

## Let's Try it Out

You’ll deploy `UEAFactoryDemo.sol`, a small **read-only** helper contract.

It reads from a predeployed system contract on Push Chain:

- **`UEAFactory`**: `0x00000000000000000000000000000000000000eA`

`UEAFactoryDemo` exposes:

- **`discoverOrigin()`**: returns `(UniversalAccountId originAccount, bool isUEA)`
- **`discoverUEAForOrigin(UniversalAccountId)`**: returns `(address uea, bool isDeployed)`

## Mental model

```mermaid

  UOA[UOA_origin_wallet]-->UEA[UEA_on_PushChain]

  UEA-->YourContract[Your_contract_on_PushChain]
```

## How to try this in Remix

1. Open <a href="https://remix.ethereum.org" target="_blank">Remix IDE</a>.
2. Create `UEAFactoryDemo.sol` and paste the contract from this folder.
3. In MetaMask, switch to **Push Chain Donut Testnet**.
4. In Remix → **Deploy & Run**, select **Injected Provider – MetaMask**.
5. Deploy `UEAFactoryDemo`.
6. Under **Deployed Contracts** call:
   - `discoverOrigin()`
   - (optional) copy the returned `UniversalAccountId` and call `discoverUEAForOrigin(...)`

### What you should see

  - an origin identity `(chainNamespace, chainId, owner)` and
  - a boolean `isUEA`

> Note: If you call this directly from MetaMask, `isUEA` will often be `false` (native Push caller).
> To see `isUEA = true`, call your contract via the universal transaction flow from the previous chapter.

## Sample responses (what they mean)

### `discoverOrigin()`

```json
[["eip155","11155111","0xfd6c2fe69be13d8be379ccb6c9306e74193ec1a9"], true]
```

- `eip155` + `11155111` → Ethereum Sepolia origin
- `0xfd6c...c1a9` → the origin wallet address on that chain
- `true` → `msg.sender` on Push Chain is a **UEA** for that origin wallet

### `discoverUEAForOrigin(UniversalAccountId)`

```json
["0x3445AEE60c70c9f5A947A28B879ca6B449B0a4ce", false]
```

- First value is the **deterministic UEA address** for that origin
- `false` means the UEA contract is **not deployed yet**

## Checkpoint (3 quick questions)

1. When your contract calls `getOriginForUEA(msg.sender)`, what does it return and what does `isUEA` tell you?
2. What’s the difference between a **UOA** and a **UEA**?
3. When is `isUEA` likely to be `false`, and what should you do to see `isUEA = true`?

## What’s next

In the next chapter (**JavaScript Account Conversion Helpers**), you’ll do the same mapping in JavaScript using SDK utilities:

- **UEA → UOA** (convert executor to origin)
- **UOA → UEA** (convert origin to executor)

This is the common pattern for dApps: resolve a user’s universal identity off-chain, then use it for app logic, analytics, and permissions.

## References

- Concepts: <a href="https://push.org/docs/chain/important-concepts/#account-types-on-push-chain" target="_blank">Account types on Push Chain</a>
- Contract Helpers: <a href="https://push.org/docs/chain/build/contract-helpers/#ueafactory--getoriginforuea" target="_blank">UEAFactory → getOriginForUEA</a>
- Contract Helpers: <a href="https://push.org/docs/chain/build/contract-helpers/#ueafactory--getueafororigin" target="_blank">UEAFactory → getUEAForOrigin</a>

