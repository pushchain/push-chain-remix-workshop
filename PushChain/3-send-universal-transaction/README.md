Learn how to use the PushChain SDK to send cross-chain Universal Transactions from Sepolia to a Simple Counter contract on the Push Chain Donut Testnet.

## Prerequisites

Before running the script, make sure you have the following in place:

- **Funding required**: The generated wallet needs native tokens (in this case Sepolia ETH) to execute transactions.
- **Get testnet tokens**: In case you are running this directly on Push Chain, you will need PC tokens to execute transactions. Visit <a href="https://faucet.push.org/" target="_blank">Push Chain Faucet</a> to get PC tokens for testing.

## What we will do

- **Create a wallet**: Generate a random Ethereum wallet on Sepolia
- **Convert to Universal Signer**: Transform the wallet into a Universal Signer instance
- **Initialize Push Chain Client**: Set up the client to interact with Push Chain Donut Testnet
- **Send Universal Transaction**: Call the `increment()` function from a smart contract deployed on Push Chain Donut Testnet from Sepolia

## Simple Counter Contract

The script interacts with the Simple Counter contract deployed at:
<a href="https://donut.push.network/address/0x5FbDB2315678afecb367f032d93F642f64180aa3?tab=index">`0x5FbDB2315678afecb367f032d93F642f64180aa3`</a>

## How it works

1. **Wallet Creation**: Creates a random wallet on Sepolia using `ethers.Wallet.createRandom()`
2. **Provider Setup**: Connects to Push Chain Donut Testnet RPC endpoint
3. **Universal Signer Conversion**: Uses `PushChain.utils.signer.toUniversal()` to create Universal Signer from the wallet
4. **Client Initialization**: Initializes Push Chain client with the Universal Signer
5. **Transaction Preparation**: Encodes the `increment()` function call using the Simple Counter ABI
6. **Funding Wait**: The code waits for Sepolia ETH to be received at the generated wallet address (using `waitForFunding` function)
7. **Transaction Execution**: Sends the transaction using `pushChainClient.universal.sendTransaction()` once funds are available

## Key Components

### Contract ABI

The script includes a minimal ABI for the Simple Counter contract:

- `increment()`: Function to increment the counter
- `reset()`: Function to reset the counter
- `countPC()`: View function to read the counter value

### Transaction Parameters

We use the `encodeTxData` helper function to encode the transaction data. This function takes the ABI and function name, then generates the proper calldata for the contract interaction. The `sendTransaction` payload follows the same structure as a standard EVM transaction payload:

```javascript
const txParams = {
  to: SIMPLE_COUNTER_CONTRACT_ADDRESS,
  value: BigInt(0),
  data: PushChain.utils.helpers.encodeTxData({
    abi: SimpleCounterABI,
    functionName: 'increment',
  }),
};
```

## Example Transactions

A few example transactions showcasing the same interaction from multiple chains:

- <a href="https://donut.push.network/tx/0xa3478c4d4034355bd3828393256e5d80806a0c45d53c67b64f0f32305e0b54de">Universal Transaction from Sepolia Ethereum</a>
- <a href="https://donut.push.network/tx/0xd3333e13729872f326f11a15020b90eba75ccbb917ff1096cf444881acb124f8">Universal Transaction from Solana Devnet</a>

**Note:** This transaction showcases calling `increment()` on the Simple Counter deployed on Push Chain from any chain, natively.

## References

- <a href="https://push.org/docs/chain/tutorials/basics/tutorial-simple-counter/" target="_blank">Official Simple Counter Tutorial</a>
- <a href="https://push.org/docs/chain/build/send-universal-transaction/" target="_blank">PushChain Documentation - Send Universal Transaction</a>
- <a href="https://www.npmjs.com/package/@pushchain/core" target="_blank">PushChain Core SDK</a>
