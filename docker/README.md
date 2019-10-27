## Setup

1. Generate X.509 certificates using [cryptogen](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/cryptogen.html):

```bash
$ cryptogen generate --config=./crypto-config.yaml 
```

2. Generate channel artifacts using [configtxgen](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/configtxgen.html):
```bash
$ mkdir -p channel-artifacts && \
  SYS_CHANNEL=toycorp \
  configtxgen \
    -profile TwoOrgsOrdererGenesis \
    -channelID $SYS_CHANNEL \
    -outputBlock ./channel-artifacts/genesis.block  
```


## Other Commands
See [Fabric Commands](https://hyperledger-fabric.readthedocs.io/en/release-1.4/command_ref.html).
