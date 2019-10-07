# GETH Private Network
https://github.com/ethereum/go-ethereum/wiki/Private-network
https://medium.com/coinmonks/ethereum-setting-up-a-private-blockchain-67bbb96cf4f1 --including multinode

## get geth & extract
```wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.9.6-bd059680.tar.gz```
```mkdir ./backend``` -- BB structure
```tar -C backend --strip 1 -xf geth-linux-amd64-1.9.6-bd059680.tar.gz```
```cd ./backend```

## init new datadir w/ genesis file
```nano init.json``` >>
{
  "config": {
    "chainId": 42101,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },

  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x400",
  "extraData"  : "",
  "gasLimit"   : "10000000",
  "alloc": {
    "0x0000000000000000000000000000000000000000": { "balance": "100000" },
    "0x0000000000000000000000000000000000000001": { "balance": "300000" },
    "0x0000000000000000000000000000000000000002": { "balance": "400000" }
  },
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```mkdir ./data```
```./geth --datadir ./data init ./init.json```

## mining
```personal.newAccount()```
```miner.setEtherbase('...')```
```miner.start(1)```
```eth.blockNumber```

## auto-mining
```./geth --datadir ./data console --mine --minerthreads=1```
