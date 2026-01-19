## Universal Counter (Solidity)

So far you’ve learned how universal execution works and how to discover an origin identity with `UEAFactory`.

Now you’ll build a small contract that makes that idea **feel real**:

> the same `increment()` function behaves differently depending on which chain the user comes from.

## Goal

Deploy `UniversalCounter.sol` on Push Chain and learn how to:

- Detect whether the caller is a **native Push Chain account** or a **UEA**
- Read the caller’s origin `(chainNamespace, chainId)`
- Track counters per origin chain (plus a total)

## What is the Universal Counter?

`UniversalCounter` is a “hello world” universal app pattern:

- One contract, deployed once (on Push Chain)
- Users from different origin chains call the same function
- The contract can keep **separate state per origin**

### What makes it “universal”

On a normal EVM chain, you usually treat `msg.sender` as “the user”.

On Push Chain, `msg.sender` can be a **UEA** (a smart account representing an external origin user). So the contract asks the chain:

- “Is this caller a UEA?”
- “If yes, what origin chain + wallet does it represent?”

## What the contract does (quick)

`UniversalCounter.sol` keeps three counters:

- `countPC`: calls made by **native Push Chain accounts**
- `countEth`: calls coming from **Ethereum Sepolia** origins
- `countSol`: calls coming from **Solana Devnet** origins

When someone calls `increment()`:

1. The contract calls `UEAFactory.getOriginForUEA(msg.sender)` at:
   `0x00000000000000000000000000000000000000eA`
2. If `isUEA == false`, it increments `countPC`.
3. If `isUEA == true`, it matches the origin chain and increments:
   - `countEth` for `("eip155", "11155111")` (Ethereum Sepolia)
   - `countSol` for `("solana", "EtWTRABZaYq6iMfeYKouRu166VU2xqa1")` (Solana Devnet)
4. If the origin chain is not one of the above, the call reverts with `"Invalid chain"`.

After each increment it emits `CountIncremented(total, caller, chainNamespace, chainId)`.

## How to try this in Remix

### Step 1: Deploy the contract

1. Open <a href="https://remix.ethereum.org" target="_blank">Remix IDE</a>.
2. Create `UniversalCounter.sol` and paste the contract from this folder.
3. In the Solidity compiler, select **0.8.30** and compile.
4. In MetaMask, switch to **Push Chain Donut Testnet**.
5. In Remix → **Deploy & Run**, select **Injected Provider – MetaMask**.
6. Deploy `UniversalCounter`.

### Step 2: Test the “native Push” path (MetaMask)

1. Under **Deployed Contracts**, call `increment()`.
2. Read:
   - `countPC` should increase
   - `countEth` and `countSol` should stay at 0

This happens because a direct MetaMask call is usually a **native Push** caller (`isUEA == false`).

### Step 3: Test the “universal” path (UEA)

To see `countEth` or `countSol` increment, you must call `increment()` using the **universal transaction flow** from Chapter 3 (origin chain signature → Push Chain execution).

When the call is executed as a UEA:

- `isUEA` is `true` inside the system
- The contract can read `(chainNamespace, chainId)` and route the increment

### What you should see

- From MetaMask: `countPC` increments.
- From a universal transaction:
  - `countEth` increments if the origin is Sepolia
  - `countSol` increments if the origin is Solana Devnet

## Checkpoint (3 quick questions)

1. Why does a MetaMask call usually increment `countPC` instead of `countEth` / `countSol`?
2. What does `UEAFactory.getOriginForUEA(msg.sender)` return, and what is the purpose of `isUEA`?
3. What happens if the origin chain is not Sepolia or Solana Devnet in this contract?

## Reference

- For a complete tutorial with a UI, see: <a href="https://push.org/docs/chain/tutorials/basics/tutorial-universal-counter/" target="_blank">Universal Counter App</a>
