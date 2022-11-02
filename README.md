## Goals of this exercise

1. Deposit collateral: ETH / WETH ✅
2. Borrow another asset: DAI
3. Repay the DAI

## Forking mainnet

1. In hardhat.config.js:

```
hardhat: {
            chainId: 31337,
            forking: {
                url: MAINNET_RPC_URL,
            },
        },
```

2. Create mainnent project in Alchemy and in `.env` specify `MAINNET_RPC_URL`

3. Default hardhat run will now run against forked mainnet

In our case, we used the WETH interface (in ./contracts/interfaces) to interact with WETH on forked mainnet, and we were able to use the live WETH mainnet address for the tx in our `getWeth.js` script
