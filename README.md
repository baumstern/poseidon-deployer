## poseidon-deployer

poseidon-deployer  is a comprehensive suite of Hardhat tasks designed for deploying [Poseidon](https://www.poseidon-hash.info) hash contract(s). It uses [circomlibjs](https://github.com/iden3/circomlibjs) to generate bytecode of Poseidon contract.

- `deploy-poseidon:callPoseidonFactory` task could deploy contract(s) by spending less gas

### Deployment Gas Cost

Deploying Poseidon contracts individually incurs the following gas costs:

    T2: 1,530,241 gas
    T3: 2,156,516 gas
    T4: 2,744,494 gas
    T5: 3,568,869 gas
    T6: 4,260,407 gas
    T7: 5,161,022 gas

Total when deployed individually: 18,761,539 gas

In contrast, deploying via poseidonFactory and multicall significantly reduces the gas costs:

    poseidonFactory: 643,259 gas
    multicall: 352,286 gas

Remarkably, multicall only uses about 1.9% of the original cost when deploying contracts individually.

## Build

Run following commands to generate a bytecode of contracts:

```
npm install
npm run build
```

## Usage

### Deploy Poseidon contract(s)

Deploy all Poseidon contracts (T2 to T7):

```
npx hardhat deploy-poseidon:EOA --network localhost
```

Deploy PoseidonT6:

```
npx hardhat deploy-poseidon:EOA --network localhost T6
```

Deploy PoseidonT2, PoseidonT4 and PoseidonT7:

```
npx hardhat deploy-poseidon:EOA --network localhost T2 T4 T7
```

### Deploy PoseidonFactory

It is required to deploy PoseidonFactory that all Poseidon contracts (T2 to T7) are deployed on-chain. The address of Poseidon contracts should be provided via `--poseidon-address-book-file-path` option:

```
npx hardhat deploy-poseidonFactory --network localhost --poseidon-address-book-file-path ./addressBook.toml
```

Required file which contains contracts address could be exported by executing `deploy-poseidon:EOA` task with `--output` option:

```
npx hardhat deploy-poseidon:EOA --network localhost --output ./addressBook.toml
```

### Deploy Poseidon contract(s) by calling PoseidonFactory

Deploy all Poseidon contracts (T2 to T7):

```
npx hardhat deploy-poseidon:callPoseidonFactory --network localhost --factory-address 0x3e58d59F9f8daadd20c0c5EF83eBF494ecdedf47 --salt 0x0000000000000000000000000000000000000000000000000000000000000cee
```

Deploy PoseidonT6:

```
npx hardhat deploy-poseidon:callPoseidonFactory --network localhost --factory-address 0x3e58d59F9f8daadd20c0c5EF83eBF494ecdedf47 --salt 0x0000000000000000000000000000000000000000000000000000000000000cee T6
```

Deploy PoseidonT2, PoseidonT4 and PoseidonT7:

```
npx hardhat deploy-poseidon:callPoseidonFactory --network localhost --factory-address 0x3e58d59F9f8daadd20c0c5EF83eBF494ecdedf47 --salt 0x0000000000000000000000000000000000000000000000000000000000000cee T2 T4 T7
```

### Testing

```
npm run test
```
