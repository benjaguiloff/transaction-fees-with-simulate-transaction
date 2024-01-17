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

### 2. Create N tokens, deploy SoroswapFactory, SoroswapRouter and create N^2 pairs.

This will create `.soroban/tokens.json`, `.soroban/factory.json`, `.soroban/pairs.json` and `.soroban/token_admin_keys.json`

Remember here to choose standalone
```
bash scripts/deploy_tokens_n_pairs.sh standalone 8 # put a even number to not to break the pair creation
```

This will:

- Create 8 test tokens.
- Build and install the SoroswapPair contract in Soroban.
- Build and Deploy the SoroswapFactory contract and initialize it with the installed SoroswapPair WASM.
- Build and Deploy the SoroswapRouter contract and initialize it with the deployed Factoy address.
- Create N^2 Pairs (all combinations between pairs) using the Router contract

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

- Replace the `contractId` const with the address found in `.soroban/router_id`.
- Replace the `path` property of the `args` const with any of the paths shown in **terminal 2** after deploying the tokens.
- Replace the `publicKey` property of the `args` const with the address found in `.soroban/token_admin_address`.
- Replace the `sourceSecretKey` const with the one found by running the following command in **terminal 2**.
  
  ```
  soroban config identity show token-admin
  ```

### 5. Run the `simulateTransaction` script

In **terminal 2**, run:

```
yarn ts-node scripts/transaction_fees/simulateTransaction.ts
```
