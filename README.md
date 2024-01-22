# transaction-fees-with-simulate-transaction
This repository serves as documentation for the use of the simulateTransaction method for the Soroban RPC, in order to get transaction fees information of a given transaction. This information will be used to compare different routes for a token swap and find the best possible path.

A new branch of the soroswap/core repository was created to explore the use of the simulateTransaction method provided by the stellar-sdk. The name of said branch is `research/transaction_fees`.

### 1. Set up:

1.1 Clone the [soroswap/core](https://github.com/soroswap/core) repository:

```
git clone http://github.com/soroswap/core.git
```

**Remember to switch to the `research/transaction_fees` branch.**

1.2 In one terminal (**terminal 1**): (for research purposes, we will work in a standalone network)

```
bash scripts/quickstart.sh standalone
```

1.3. In another terminal (**terminal 2**)

```
bash scripts/run.sh
```

1.4 yarn install

```
yarn
```

### 2. Deploy populated network

In the same terminal mentioned before, run:

```

    bash scripts/populate_network.sh standalone

```


This will:  
- Create 8 test tokens.

- Build and install the SoroswapPair contract in Soroban.

- Build and Deploy the SoroswapFactory contract and initialize it with the installed SoroswapPair WASM.

- Build and Deploy the SoroswapRouter contract and initialize it with the deployed Factoy address.

- Create 64 Pairs (all combinations between pairs) using the Router contract.

- Create 6 custom liquidity pools.

- Create `.soroban/tokens.json`, `.soroban/factory.json`, `.soroban/pairs.json` and `.soroban/token_admin_keys.json`  
  

This will create the `.soroban` folder with a lot of useful `.json` files with the contract and admin addresses.



### 3. (For local development): Serve those .json files

In a new terminal (**terminal 3**) run

```
bash scripts/serve_with_docker.sh
```

This will serve:

- List of tokens at http://localhost:8010/api/tokens
- Factory addresses http://localhost:8010/api/factory
- Router addresses http://localhost:8010/api/router
- Admin keys http://localhost:8010/api/keys
- Created pairs http://localhost:8010/api/keys

### 4. Replace the corresponding addresses in `scripts/transaction_fees/simulateTransaction.ts`

`contractId`, `publicKey` and `sourceSecretKey` are directly read from the `.soroban` folder. The only thing that still has to be manually replaced is the following:

- Replace the `path` property of the `args` const with any of the paths shown in **terminal 2** after deploying the tokens.

### 5. Run the `simulateTransaction` script

In **terminal 2**, run:

```
yarn ts-node scripts/transaction_fees/simulateTransaction.ts
```

This script simulates a transaction that contains a "swap_exact_tokens_for_tokens". This can be changed or extended if needed to match the needed operations of the transaction being simulated.
