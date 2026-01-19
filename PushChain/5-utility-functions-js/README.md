An overview of helper functions that demonstrate the <a href="https://www.npmjs.com/package/@pushchain/core">`@pushchain/core SDK`</a> utilities for mapping between Origin Accounts (UOA) and Executor Accounts (UEA).

## `utility-functions.js`

This script now uses high-level SDK utilities instead of calling the on-chain factory directly:

### `PushChain.utils.account.convertExecutorToOriginAccount(ueaAddress)`

- **Purpose**: Given a UEA (executor) address on Push Chain, return the origin account (UOA).
- **Input**: `ueaAddress: string`
- **Returns**:
  - `account`: `{ chain: string, address: string }`
  - `exists`: `boolean`

### `PushChain.utils.account.convertOriginToExecutor(universalAccount)`

- **Purpose**: Given a `UniversalAccount` (origin), compute the deterministic UEA and check its deployment status.
- **Input**: `universalAccount` returned by `PushChain.utils.account.toUniversal(address, { chain })`
- **Returns**:
  - `address`: `string`
  - `deployed`: `boolean`

## References

- Docs: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/utility-functions/#convert-executor-address-to-origin-account" target="_blank">Utility Functions → Convert Executor Address to Origin Account</a>
- Docs: <a href="https://pushchain.github.io/push-chain-website/pr-preview/pr-1067/docs/chain/build/utility-functions/#convert-origin-to-executor-account" target="_blank">Utility Functions → Convert Origin to Executor Account</a>
