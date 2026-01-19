Push Chain is a True Universal Layer 1 that is 100% EVM-compatible. In practice, deploying to Push Chain looks just like deploying to any other EVM chain: you write a normal Solidity contract, compile it, and choose Push Chain as the target network.

In this chapter you will learn the minimum theory you need before writing code:

- **Account types on Push Chain**
- **UEAs** and why they exist
- **Universal Transactions** (sign on origin chain → execute on Push Chain)

If you want to go deeper later, keep these doc links handy:

- <a href="https://push.org/docs/chain/" target="_blank">Intro to Push Chain</a>
- <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/important-concepts/" target="_blank">Important Concepts</a>

## Account types (high level)

Push Chain supports the standard Ethereum-style accounts you already know:

- **Externally Owned Accounts (EOAs)**<br />
  Regular private‑key‑controlled addresses (for example, MetaMask wallets).

- **Smart Contract Accounts (Smart Accounts)**<br />
  On-chain contracts that hold logic (for example, multisigs, social recovery wallets).

For this workshop, two universal identity concepts matter:

- **Universal Origin Account (UOA)**<br />
  The user’s original wallet identity on their origin chain (for example, an Ethereum Sepolia EOA or a Solana wallet).

- **Universal Executor Account (UEA)**<br />
  A smart account on Push Chain that represents that origin user when execution happens on Push Chain.

In other words: **the UOA is who the user is** on their home chain; **the UEA is how that user shows up** on Push Chain.

Learn more in <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/important-concepts/#account-types-on-push-chain" target="_blank">Important Concepts → Account Types on Push Chain</a>.

## Universal transactions (high level)

Universal transactions let a user **sign on their origin chain** while your contract **executes on Push Chain**.

When you send a universal transaction, the SDK:

1. Detects the user’s origin chain.
2. Creates/uses the user’s UEA on Push Chain.
3. Routes the call so it executes on Push Chain as the UEA.

You focus on application logic instead of building per‑chain deployments, bridges, and custom routing.

### Why Universal Transactions
Universal Transactions comes with many benefits, such as: 
- **Cross-chain compatibility**: Send transactions from any chain (EVM or non-EVM).
- **No bridging required**: Direct native transactions without wrapping or bridging.
- **Simplified development**: No need for complex interoperability tooling.

Learn more in <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/send-universal-transaction/" target="_blank">Send Universal Transaction</a>.

## Checkpoint (quick self-check)

1. What is the difference between a **UOA** and a **UEA**?
2. In a universal transaction, where does the user **sign** and where does the contract **execute**?
3. Why does the UEA exist (what problem does it solve for your contracts/dApps)?

Next up: deploying your first Solidity contract to Push Chain from Remix.
