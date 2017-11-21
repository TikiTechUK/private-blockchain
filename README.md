# private-blockchain

the point of this repo is to document how to setup a private ethereum block-chain for development purposes.

## setup

For this you will need to install [geth](https://github.com/ethereum/go-ethereum)


## create a folder

```
$ mkdir privatenetwork
$ cd privatenetwork
$ export ethereum_home=$PWD
```

## create the genesis block config

Now add the following JSON to a `genesis.json` file (inside `$ethereum_home`)

```json
{
    "config": {
        "chainId": 100,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "400",
    "gasLimit": "2100000",
    "alloc": {
        "7df9a875a174b3bc565e6424a0050ebc1b2d1d82": { "balance": "300000" },
        "f41c74c9ae680c1aa78f42e5647a62f353b7bdde": { "balance": "400000" }
    }
}
```

## initialize command

Now we initialize the genesis block that will use the config we just created:

```bash
$ geth --datadir "$ethereum_home" init "$ethereum_home/genesis.json"
```

## run javascript console

Then we run the geth console:

```bash
$ geth --datadir "$ethereum_home" console --networkid 100
```

## view admin outout

Now we can dump the admin info for the server - NOTE - make sure you can see the `>` meaning we are in the console:

```bash
> admin
{
  datadir: "/Users/kai/projects/private-blockchain/privatenetwork/privatenetwork",
  nodeInfo: {
    enode: "enode://07c25495068df19102c0e2a62f9e5b183becb12893a5b3316fb41270c5d6bb9773397ff09454e13f6b18bcac518e8e745df28e090bf87c43fb5641390d394c9a@[::]:30303",
    id: "07c25495068df19102c0e2a62f9e5b183becb12893a5b3316fb41270c5d6bb9773397ff09454e13f6b18bcac518e8e745df28e090bf87c43fb5641390d394c9a",
    ip: "::",
    listenAddr: "[::]:30303",
    name: "Geth/v1.7.0-stable-6c6c7b2a/darwin-amd64/go1.9",
    ports: {
      discovery: 30303,
      listener: 30303
    },
    protocols: {
      eth: {
        difficulty: 400,
        genesis: "0xa7aac261cc19872089526fb17580ac6406bda1326cb86f9a9c04eae4b8ba86f5",
        head: "0xa7aac261cc19872089526fb17580ac6406bda1326cb86f9a9c04eae4b8ba86f5",
        network: 1
      }
    }
  },
  peers: [],
  addPeer: function(),
  exportChain: function(),
  getDatadir: function(callback),
  getNodeInfo: function(callback),
  getPeers: function(callback),
  importChain: function(),
  removePeer: function(), 
  sleep: function github.com/ethereum/go-ethereum/console.(*bridge).Sleep-fm(),
  sleepBlocks: function github.com/ethereum/go-ethereum/console.(*bridge).SleepBlocks-fm(),
  startRPC: function(),
  startWS: function(),
  stopRPC: function(),
  stopWS: function()
}
```

## check block number

Type the following into the console to ensure we are starting with zero blocks:

```bash
> eth.blockNumber
0
```

## run geth server

Run a single geth server on ONE node:

```bash
$ geth --datadir "$ethereum_home" --networkid 100
```

This will have created a listening server running on `127.0.0.1:30303`

You can test this:

```bash
$ telnet 127.0.0.1 30303
```

Grab the `UDP listener up` line and copy the `enode://XXX@[::]:30303` line.

Find out your local ip address (`$ ifconfig | grep 192`) - and replace `[::]` with that ip:

e.g. here is a bootnode address:

```
enode://165ec598b922a9ab8b195a098afe2524fecd8b23edb758ceeca064299d72b4c037f504af84f2cbec494094f23734ca0490dc9e4b437efe92efee261423625e04@192.168.0.35:30303
```

This is used to created the `geth` command to be run on the other nodes using the `--bootnodes` flag - for example:

```bash
$ geth --datadir "$ethereum_home" --networkid 100 --bootnodes enode://165ec598b922a9ab8b195a098afe2524fecd8b23edb758ceeca064299d72b4c037f504af84f2cbec494094f23734ca0490dc9e4b437efe92efee261423625e04@192.168.0.35:30303
```




