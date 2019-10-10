# Prereqs (Windows)
## Docker Windows -> WSL
https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly
https://medium.com/faun/docker-running-seamlessly-in-windows-subsystem-linux-6ef8412377aa
https://github.com/docker/for-win/issues/3570

# GETH Private Network
https://github.com/ethereum/go-ethereum/wiki/Private-network
https://medium.com/coinmonks/ethereum-setting-up-a-private-blockchain-67bbb96cf4f1 --including multinode

## get geth & extract
```wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.9.6-bd059680.tar.gz```
```mkdir ./backend``` -- BB structure
```tar -C backend --strip 1 -xf geth-linux-amd64-1.9.6-bd059680.tar.gz```
```cd ./backend```

## init new datadir w/ genesis file
For alloc, we need to create accounts before init'ing the new chain: https://github.com/ethereum/go-ethereum/issues/14831
```geth --datadir ./data1 account new```
  > outputs, e.g. 0xE3cadd6A998C762d827364280a7827cb1924C866 + ./data1/keystore... (privkey)
```geth --datadir ./data1 account new```
> outputs, e.g. 0xE3cadd6A998C762d827364280a7827cb1924C866 + ./data1/keystore... (privkey)

```nano init.json``` >> (alloc: 1000 ETH, 1 ETH, 1 ETH)
{
  "config": {
    "chainId": 42101,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x400",
  "extraData"  : "AIRCARBON",
  "gasLimit"   : "10000000",
  "alloc": {
    "0xE3cadd6A998C762d827364280a7827cb1924C866": { "balance": "1000000000000000000000" },
    "0x6EF0168c5F00dD84A5ec6c0533595dD311B10FB7": { "balance": "1000000000000000000" }
  },
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```mkdir ./data1```
```./geth --datadir ./data1 init ./init.json```

## mining
```personal.newAccount()```
```miner.setEtherbase('...')```
```miner.start(1)```
```eth.blockNumber```
```eth.getBalance('0x0000000000000000000000000000000000000000')```
```eth.accounts```debug.

## auto-mining
NOTE: 0x0000000000000000000000000000000000000000 is an invalid etherbase
--verbosity 2 (warn) --verbosity 1 (err) --verbosity 0 (silent) == debug.verbosity(x)
```./geth --datadir ./data1 console --etherbase '0xE3cadd6A998C762d827364280a7827cb1924C866' --mine --minerthreads=1 --verbosity 2```

## send tx
```personal.unlockAccount('0xe3cadd6a998c762d827364280a7827cb1924c866')```
```eth.sendTransaction({from:"0xE3cadd6A998C762d827364280a7827cb1924C866", to:"0x6EF0168c5F00dD84A5ec6c0533595dD311B10FB7", value: web3.toWei(1, "ether")})```
```eth.getTransaction('0xfc8a7d4938c443fd12a960233f9732c980ee12bb793d902b247ebff3f5c9f57c')```
  