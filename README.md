# Private Chain

## Run from docker

### Boot node
```console
cd network/node1
```
```console
docker compose run node1
```

###  Copy the node hash from enode URL in terminal
```console
private-chain-node1-1  | 2023-04-05 11:50:47.406+00:00 | main | INFO  | DefaultP2PNetwork | Enode URL enode://0b1e55676ef98d25f8a7dcfbd8464b6806a780a7260ee5d2c6715f2abeb0e6a0acdfca00098da00cf7d567f7ba2dcafcd4523c3ea3b9ee4574617ebeb2127371@127.0.0.1:30303
```

### Stop node 1
```console
docker compose stop node1
```

### Create .env file

Specifying the node hash and the host in .env file :
```
HOST=127.0.0.1
NODE=0b1e55676ef98d25f8a7dcfbd8464b6806a780a7260ee5d2c6715f2abeb0e6a0acdfca00098da00cf7d567f7ba2dcafcd4523c3ea3b9ee4574617ebeb2127371
```

### Start docker
```console
docker compose up
```

### Explorer Url
http://localhost


## Create from config

https://besu.hyperledger.org/en/stable/private-networks/tutorials/qbft/#2-create-a-configuration-file

### Generate node keys and a genesis file
```console
besu operator generate-blockchain-config --config-file=config/qbft.json --to=networkFiles --private-key-file-name=key
```

### Copy config to network
```console
cp networkFiles/genesis.json network
```

### Copy the node private keys to the node directories
```html
network/
├── genesis.json
├── node1
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── node2
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── node3
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── node4
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── node5
│   ├── data
│   │    ├── key
│   │    ├── key.pub
```

### Start the first node as the bootnode
```console
cd network/node1
```
```console
besu --data-path=data --genesis-file=../genesis.json --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all"
```

### Start the node2
Start another terminal, change to the node2 directory and start node2 specifying the node1 enode URL copied when starting node1 as the bootnode:
```console
cd network/node2
```
```console
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<node1 enode URL> --p2p-port=30304 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8546
```

### Start the node3
Start another terminal, change to the node3 directory and start node3 specifying the node1 enode URL copied when starting node1 as the bootnode:
```console
cd network/node3
```
```console
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<node1 enode URL> --p2p-port=30305 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8547
```

### Start the node4
Start another terminal, change to the node4 directory and start node4 specifying the node1 enode URL copied when starting node1 as the bootnode:
```console
cd network/node4
```
```console
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<node 1 enode URL> --p2p-port=30306 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8548
```

### Start the node5
Start another terminal, change to the node5 directory and start node5 specifying the node1 enode URL copied when starting node1 as the bootnode:
```console
cd network/node5
```
```console
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<node1 enode URL> --p2p-port=30307 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8549
```

### Confirm the private network is working
```console
curl -X POST --data '{"jsonrpc":"2.0","method":"qbft_getValidatorsByBlockNumber","params":["latest"], "id":1}' localhost:8545
```

## Add to metamask
https://besu.hyperledger.org/en/stable/private-networks/tutorials/quickstart/#create-a-transaction-using-metamask
