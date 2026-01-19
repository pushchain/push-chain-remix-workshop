You’ve finished the Push Chain Remix workshop.
This page summarizes what you built, what you learned, and what to do next.

## What you built (end-to-end)

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

1. **Universal subscription + paywall (SaaS for web3)**
   - Build a single on-chain subscription registry on Push Chain.
   - Users can subscribe from different origin chains, but you track entitlements by **UOA** (not by per-chain wallets).
   - Use origin chain as a pricing lever (e.g., discounts per chain, region/market segmentation).

2. **Multi-chain reputation + sybil resistance registry**
   - Record reputation score, badges, or attestations in one universal contract.
   - When a user interacts from any chain, resolve their origin identity and update/verify reputation under the same user record.
   - Useful for gated mints, airdrops, governance access, or community moderation.

3. **Universal airdrop/claims engine (one contract, many chains)**
   - A single claims contract on Push Chain where eligibility is keyed by **UOA**.
   - Users claim from their origin chain without needing bridges/wrappers.
   - Great for ecosystem launches: one airdrop pipeline, many communities.

4. **Cross-chain loyalty + rewards program for dApps**
   - Track user activity and points in one place even if users interact from multiple chains/wallets.
   - Prevent double counting by keying rewards to **UOA**.
   - Add “origin-aware rewards”: boost points for specific origin chains (campaigns).

5. **Universal access control for contracts + on-chain apps**
   - Build a reusable “universal ACL” contract: roles/permissions keyed by **UOA**.
   - Other Push Chain contracts consult it to decide who can call what.
   - This is useful for DAOs, multi-chain teams, compliance rules, admin actions, and feature gating.

## Resources and doc links

- **Docs home**: <a href="https://push.org/docs/chain/" target="_blank">Push Chain Docs</a>
- **Important Concepts**: <a href="https://push.org/docs/chain/important-concepts/" target="_blank">Important Concepts</a>
- **Contract helpers** (`UEAFactory`, etc.): <a href="https://push.org/docs/chain/build/contract-helpers/" target="_blank">Contract Helpers</a>
- **Send universal transaction**: <a href="https://push.org/docs/chain/build/send-universal-transaction/" target="_blank">Send Universal Transaction</a>
- **Create universal signer**: <a href="https://push.org/docs/chain/build/create-universal-signer/" target="_blank">Create Universal Signer</a>
- **Initialize Push Chain client**: <a href="https://push.org/docs/chain/build/initialize-push-chain-client/" target="_blank">Initialize Push Chain Client</a>
- **UI Kit (for frontends)**: <a href="https://push.org/docs/chain/ui-kit/integrate-push-universal-wallet/" target="_blank">Integrate Push Universal Wallet</a>
- **Universal Counter App (full UI tutorial)**: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/tutorials/basics/tutorial-universal-counter/" target="_blank">Universal Counter App</a>