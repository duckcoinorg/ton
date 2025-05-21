# TON JS Client

[![Version npm](https://img.shields.io/npm/v/@duckcoin/ton.svg?logo=npm)](https://www.npmjs.com/package/@duckcoin/ton)

Cross-platform client for TON blockchain.

## Features

- 🚀 Create new wallets
- 🍰 Get balance
- ✈️ Transfers

### Planned
- 🌎 DNS (including custom)

## Install

```bash
yarn add @duckcoin/ton @ton/crypto @ton/core buffer
```

## Test

If you experience issues with tests failing with 500 errors - consider using different API or if it's not feasible - you can replace those with warnings by calling `yarn test` with `IGNORE_500=true` as an environment variable

### Disclaimer 🐛🐞

That's a temporary solution. We plan to resolve those with splitting suites into `unit` and `e2e` correspondingly and negotiating with API providers in order to find out and solve the root cause

#### Browser polyfill

```js
// Add before using library
require("buffer");
```

## Usage

To use this library you need HTTP API endpoint, you can use one of the public endpoints:

- Mainnet: https://toncenter.com/api/v2/jsonRPC
- Testnet: https://testnet.toncenter.com/api/v2/jsonRPC

```ts
import { TonClient, WalletContractV4, internal } from "@duckcoin/ton";
import { mnemonicNew, mnemonicToPrivateKey } from "@ton/crypto";

// Create Client
const client = new TonClient({
  endpoint: 'https://toncenter.com/api/v2/jsonRPC',
});

// Generate new key
let mnemonics = await mnemonicNew();
let keyPair = await mnemonicToPrivateKey(mnemonics);

// Create wallet contract
let workchain = 0; // Usually you need a workchain 0
let wallet = WalletContractV4.create({ workchain, publicKey: keyPair.publicKey });
let contract = client.open(wallet);

// Get balance
let balance: bigint = await contract.getBalance();

// Create a transfer
let seqno: number = await contract.getSeqno();
let transfer = await contract.createTransfer({
  seqno,
  secretKey: keyPair.secretKey,
  messages: [internal({
    value: '1.5',
    to: 'EQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAM9c',
    body: 'Hello world',
  })]
});

```

## Docs

[Documentation](https://duckcoinorg.github.io/ton/)

## Acknowledgements

This repository is currently maintained by [Duck 🦆](https://docs.duckcoin.org/)

This library was initially developed by the [Whales Corp.](https://tonwhales.com/) and maintained by [Dan Volkov](https://github.com/dvlkv).

## License

MIT
