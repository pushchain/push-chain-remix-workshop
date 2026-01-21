You’re a Solidity developer.
You want to build and deploy smart contracts.
You must also handle the operations of your deployed onchain contracts. 

But its getting increasingly difficult as we get more chains. 

That exactly where Push Chain helps YOU.

### Before Push Chain

As more chains and wallets become mainstream, “shipping a dApp everywhere” usually means:

- Multiple deployments (one per chain)
- Different wallet flows / SDKs (EVM vs non‑EVM)
- Bridges, wrappers, and custom plumbing
- Fragmented UX (users switch networks, acquire new gas tokens, and drop off)

Over time, this doesn’t get easier — it gets harder.

### Why Push Chain

- **Deploy once, reach many chains**: Deploy your Solidity contract once on Push Chain and let users interact from multiple origin chains (EVM and non-EVM both).
- **No contract rewrites**: Push Chain is **100% EVM-compatible** with same Solidity, ABI, tooling.
- **Better UX for users**: Users can keep using familiar wallets (like MetaMask or Phantom) without you rebuilding everything per chain.

### How Push Chain makes it easy for you

Push Chain introduces a **universal execution model**:

- Users **sign on their origin chain**
- Execution happens **on Push Chain**
- The system represents origin users on Push Chain via smart accounts, so you can keep your contract logic simple

This removes the need for you to manually stitch together per‑chain deployments and custom interoperability code.

### By the end of this workshop, you will

- Know what Push Chain is for (and its USP)
- Deploy a Solidity contract on Push Chain from Remix
- Send a universal transaction (origin chain signature → Push Chain execution)
- Understand the basics of UOA/UEA and how to detect origin in Solidity

Next up: a brief intro to Push Chain core concepts, then we’ll start deploying contracts.
