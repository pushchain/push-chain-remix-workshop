Push Chain is a True Universal Layer 1 that is 100% EVM-compatible. In practice, deploying to Push Chain looks just like deploying to any other EVM chain: you write a normal Solidity contract, compile it, and choose Push Chain as the target network.

In this chapter you will:

- **Add Push Chain Donut Testnet to your wallet (MetaMask)**
- **Deploy a simple Solidity contract from Remix to Push Chain**
- **Confirm the deployment on the Push Chain block explorer**

Learn more background concepts in the official docs: <a href="https://push.org/docs/chain/" target="_blank">Intro to Push Chain</a>.

## Step 1: Add Push Chain Donut Testnet to MetaMask

1. Open MetaMask and click the network selector.
2. Click **Add network** (or **Add network manually**).
3. Fill in the following values:
   - **Network Name**: `Push Chain Donut Testnet`
   - **RPC URL**: `https://evm.donut.rpc.push.org/`
   - **Chain ID**: `42101`
   - **Currency Symbol**: `PC`
   - **Block Explorer URL**: `https://donut.push.network`
4. Save the network and switch MetaMask to **Push Chain Donut Testnet**.
5. Visit the faucet to get test PC for gas: <a href="https://faucet.push.org/" target="_blank">Push Chain Faucet</a>.

> **Note:** You do **not** need to change your Solidity code to deploy on Push Chain. Any contract that compiles for a standard EVM chain will compile and deploy the same way here.

## Step 2: Write a simple contract in Remix

1. Go to <a href="https://remix.ethereum.org" target="_blank">Remix IDE</a>.
2. Create a new file named `HelloPush.sol` in your workspace.
3. Paste this minimal Solidity contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.22;

contract HelloPush {
    uint256 public value;

    function set(uint256 _value) external {
        value = _value;
    }
}
```

4. In the **Solidity compiler** tab, choose compiler version **0.8.22** (or any compatible 0.8.x version) and click **Compile HelloPush.sol**.

## Step 3: Deploy to Push Chain from Remix

1. Open the **Deploy & Run Transactions** tab in Remix.
2. Set **Environment** to **Injected Provider - MetaMask**. Remix will connect to the network currently selected in MetaMask.
3. Confirm that MetaMask is on **Push Chain Donut Testnet**.
4. Select the `HelloPush` contract in the **Contract** dropdown.
5. Click **Deploy** and confirm the transaction in MetaMask.
6. After the transaction is mined, Remix will show the deployed contract instance under **Deployed Contracts**.
7. Copy the contract address and open it in the explorer:

   - `https://donut.push.network/address/YOUR_CONTRACT_ADDRESS`

You’ve now deployed a standard Solidity contract to Push Chain Donut Testnet with **no on-chain code changes**.

## Push Chain Donut Testnet configuration (reference)

Use the following network details when deploying to Push Chain Donut Testnet. See the full specs in the docs: <a href="https://push.org/docs/chain/setup/chain-config/" target="_blank">Chain Configuration</a>.

- **Network Name**: Push Chain Donut Testnet
- **RPC URL**: `https://evm.donut.rpc.push.org/`
- **Chain ID**: `42101`
- **Currency Symbol**: `PC`
- **Block Explorer**: `https://donut.push.network`
- **Faucet**: `https://faucet.push.org/`

If you prefer deploying with Hardhat or Foundry instead of Remix, follow the official guides:

- **Configure Hardhat**: <a href="https://push.org/docs/chain/setup/smart-contract-environment/configure-hardhat/" target="_blank">Push Chain Docs — Configure Hardhat</a>
- **Configure Foundry**: <a href="https://push.org/docs/chain/setup/smart-contract-environment/configure-foundry/" target="_blank">Push Chain Docs — Configure Foundry</a>
