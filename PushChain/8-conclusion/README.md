You didn’t just complete a workshop.

You deployed **one Solidity contract** that can be executed by users from **multiple blockchains**, without redeploying, bridging, or changing your code.

That is the point of Push Chain.


## What you actually did

- **Deployed contracts on Push Chain** from Remix (Donut Testnet)
- **Sent a universal transaction** (sign on an origin chain → execute on Push Chain)
- **Used on-chain helpers** (`UEAFactory`) to discover the user’s origin identity
- **Built a Universal Counter** that keeps different state depending on the caller’s origin chain

## What you learned (key concepts to take with you)

- **Universal execution model**: users can sign on their origin chain, but your contract runs on Push Chain.
- **UOA vs UEA**:
  - **UOA (Universal Origin Account)** = who the user is on their home chain
  - **UEA (Universal Executor Account)** = how that user appears on Push Chain during execution
- **Origin-aware contracts**: your Solidity can branch behavior using `(chainNamespace, chainId)` instead of treating all callers the same.
- **UEAFactory mental model**:
  - `getOriginForUEA(address)` answers “who is this caller really?”
  - `getUEAForOrigin(origin)` answers “what is the deterministic executor account for this origin?”

## Quiz: TEST Yourself 

1. What does “universal execution” mean in Push Chain, in one sentence?
2. What is the difference between **UOA** and **UEA**?
3. Why might `isUEA` be `false` when you call a contract from MetaMask in Remix?
4. What does `UEAFactory.getOriginForUEA(msg.sender)` return (conceptually)?
5. What does it mean that a UEA address is **deterministic**?
6. In `UniversalCounter`, what decides whether `countPC` vs `countEth` / `countSol` increments?
7. If you were building an allowlist, would you key it by UEA or UOA? Why?

> If you answered all of these questions correctly, CONGRATULATIONS. You are all set to build apps on Push Chain.

## What to build next (5 useful app ideas)

These ideas are designed to **use universal smart contracts for real product UX**, not just demos. The pattern in all of them is the same: deploy once on Push Chain, then make decisions using the user’s **origin identity (UOA)**.

1. **Universal DeFi + Lending**
    - Build DeFi that can be used by users of any chain. 
    - Pool liquidity or assets from every chain.
    - Create a DEX that can trade assets from every chain.
 
2. **Universal payments + subscription + paywall (SaaS for web3)**
   - Allow users to pay for your app from any chain.
   - Build a single on-chain subscription registry on Push Chain.
   - Users can subscribe from different origin chains, but you track entitlements by **UOA** (not by per-chain wallets).

3. **Universal airdrop/claims engine (one contract, many chains)**
   - A single claims contract on Push Chain where eligibility is keyed by **UOA**.
   - Users claim from their origin chain without needing bridges/wrappers.
   - Great for ecosystem launches: one airdrop pipeline, many communities.

4. **RWA (Real World Assets) or Tokenization**
   - Build RWA that allows users to access, buy or trade these assets from any chain.
   - Launch Tokenized Assets on Push Chain and allow users to access it from any chain.

5. **Gaming + NFTs**
   - Allow users to access, buy or trade these assets from any chain.
   - Launch Tokenized Assets on Push Chain and allow users to access it from any chain.

## Resources and doc links

- **Docs home**: <a href="https://push.org/docs/chain/" target="_blank">Push Chain Docs</a>
- **Important Concepts**: <a href="https://push.org/docs/chain/important-concepts/" target="_blank">Important Concepts</a>
- **Contract helpers** (`UEAFactory`, etc.): <a href="https://push.org/docs/chain/build/contract-helpers/" target="_blank">Contract Helpers</a>
- **Send universal transaction**: <a href="https://push.org/docs/chain/build/send-universal-transaction/" target="_blank">Send Universal Transaction</a>
- **Create universal signer**: <a href="https://push.org/docs/chain/build/create-universal-signer/" target="_blank">Create Universal Signer</a>
- **Initialize Push Chain client**: <a href="https://push.org/docs/chain/build/initialize-push-chain-client/" target="_blank">Initialize Push Chain Client</a>
- **UI Kit (for frontends)**: <a href="https://push.org/docs/chain/ui-kit/integrate-push-universal-wallet/" target="_blank">Integrate Push Universal Wallet</a>
- **Universal Counter App (full UI tutorial)**: <a href="https://push.org/docs/chain/tutorials/basics/tutorial-universal-counter/" target="_blank">Universal Counter App</a>