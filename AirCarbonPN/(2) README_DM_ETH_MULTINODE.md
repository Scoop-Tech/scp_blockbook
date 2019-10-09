# Multinode setup

## node1 >> specify a network id (why?, it's in the init.json?!)
```./geth --datadir ./data1 console --etherbase '0xE3cadd6A998C762d827364280a7827cb1924C866' --mine --minerthreads=1 --networkid 42101```
```admin.nodeInfo``` - get enode value...

## node2 instance >> new console ... (new datadir, same init.json) - specify enode for peer discovery
```./geth --datadir ./data2 init init.json```
```./geth --datadir ./data2 --networkid 42101 --port 30306 console --bootnodes enode://c9181a166bf931ef56bd580a82c94f551d65132b08eaa3425252bbe43fd71cab8e144b97fc6df04814f0d3dedc616b1d320040b71755e062858a02265254c265@127.0.0.1:30303```
```admin.peers```

## node3 (BB) >> install (don't run yet!) ethereum_aircarbon.json template from scp-blockbook
```make all-ethereum_aircarbon```
```dpkg --remove backend-ethereum-ac.service```
```rm -rf /opt/coins```
```apt install ./build/backend-ethereum-ac_1.9.6-bd059680-satoshilabs-1_amd64.deb --reinstall```

## node3 (BB) >> post-install: init w/ AC genesis file
>>> wget https://raw.githubusercontent.com/Scoop-Tech/scp-blockbook/master/AirCarbonPN/init.json -O {{.Env.BackendInstallPath}}/{{.Coin.Alias}}/init.json
```/opt/coins/nodes/ethereum-ac//geth --datadir /opt/coins/data/ethereum-ac/backend init /opt/coins/nodes/ethereum-ac/init.json```



### bootnode -- red herring: spins up strict dev networks (default init.jsons) - not so useful
### Install bootnode
https://ethereum.stackexchange.com/questions/44125/bootnode-command-not-found
https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster
```sudo add-apt-repository -y ppa:ethereum/ethereum```
```sudo apt-get update```
```sudo apt-get install bootnode```
```bootnode -genkey bootnode.key```
```bootnode -nodekey bootnode.key``` -- spins up cutdown instance