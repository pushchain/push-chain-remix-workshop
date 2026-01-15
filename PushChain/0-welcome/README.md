You’re a Solidity developer who wants your contracts to reach users everywhere without rebuilding them for every new chain. Push Chain is a universal, EVM‑compatible Layer 1 that lets you **deploy your Solidity contract once and have it executed from EVM and non‑EVM chains**.

## Why Push Chain matters to you

- **Single deployment, multi‑chain reach**: Deploy your existing Solidity contract once on Push Chain and instantly reach users from Ethereum, Base, Arbitrum, Optimism, Solana, and other supported chains—without managing separate deployments or custom bridges.
- **No contract rewrites (EVM compatible)**: Keep your existing Solidity, ABI, and bytecode. Push Chain is 100% EVM-compatible, so you don’t need to change on‑chain code or re‑architect your contracts to deploy here.
- **Wallet and fee abstraction for your users**: Users connect using familiar wallets like MetaMask or Phantom and pay gas in the native token of their home chain; they never need to hold a special Push Chain gas token.

## Fee abstraction and cross‑chain execution

On Push Chain, users don’t need to hold the native Push Chain token to use your app. They:

1. Sign a transaction on their home chain (for example, an EVM or non‑EVM chain they already use).
2. Pay gas in that chain’s native token.
3. Have your contract executed on Push Chain behind the scenes.

Under the hood, Push Chain creates and manages a smart account for each external user and uses their locked gas funds to execute your contract on Push Chain. **From the user’s perspective, they just use their usual wallet on their usual chain.**

Learn more in <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/important-concepts/#fee-abstraction-and-cross-chain-execution" target="_blank">Important Concepts → Fee Abstraction and Cross-Chain Execution</a>.

## Account types (high level)

Push Chain supports the standard Ethereum-style accounts you already know:

- **Externally Owned Accounts (EOAs)**<br />
  Regular private‑key‑controlled addresses (for example, MetaMask wallets).

- **Smart Contract Accounts (Smart Accounts)**<br />
  On-chain contracts that hold logic (for example, multisigs, social recovery wallets).

For this tutorial, one new account type really matters:

- **Universal Executor Accounts (UEAs)**<br />
  A smart account on Push Chain that represents a user coming from an external chain. When a user first interacts with your app from their home chain, a UEA is created for them on Push Chain and is used to execute your contract logic.

Each UEA is linked to the user’s original wallet, called a **Universal Origin Account (UOA)**. UOAs let you attribute activity back to the user’s home chain while interacting with a single account on Push Chain.

Learn more in <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/important-concepts/#account-types-on-push-chain" target="_blank">Important Concepts → Account Types on Push Chain</a>.

## Universal transactions

Universal transactions let a user **sign a transaction on their home chain while your contract runs on Push Chain**. You write and deploy your Solidity contract once, and the Push Chain SDK takes care of the cross‑chain plumbing.

When you send a universal transaction, the SDK:

1. Detects the user’s origin chain.
2. Estimates gas costs and orchestrates the necessary signatures.
3. Routes the call so it executes on Push Chain via the user’s Universal Executor Account (UEA).

You focus on your application logic instead of bridging, wrapping, or managing separate deployments per chain.

Learn more in <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/send-universal-transaction/" target="_blank">PushChain Documentation - Send Universal Transaction</a>.
