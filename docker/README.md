## Setup

#### 1. Generate X.509 certificates using [cryptogen](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/cryptogen.html)

```bash
$ cryptogen generate --config=./crypto-config.yaml 
```

#### 2. Generate channel artifacts using [configtxgen](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/configtxgen.html)
```bash
$ mkdir -p channel-artifacts && \
  SYS_CHANNEL=toycorp \
  configtxgen \
    -profile TwoOrgsOrdererGenesis \
    -channelID $SYS_CHANNEL \
    -outputBlock ./channel-artifacts/genesis.block  
```

#### 3. Generate Fabric CA docker containers

- `IMAGE_TAG`: See [hyperledger/fabric-ca](https://hub.docker.com/r/hyperledger/fabric-ca/tags) tags.
- The private keys are found in `ca` directory under each `crypto-config/peerOrganizations/<org>`.

```bash
$ IMAGE_TAG=1.4 \
  SALES_CA_PRIVATE_KEY=somehash_sk \
  ACCOUNTING_CA_PRIVATE_KEY=somehash_sk \
  LEGAL_CA_PRIVATE_KEY=somehash_sk \
  docker-compose -f docker-compose-ca.yaml up -d
```

Verify running CA containers
```
$ docker ps -a
CONTAINER ID        IMAGE                            ...    NAMES
133b61a2f9f3        hyperledger/fabric-ca:1.4        ...    ca.accounting.toycorp.com
93367388ab19        hyperledger/fabric-ca:1.4        ...    ca.sales.toycorp.com
66c89dbe5f3d        hyperledger/fabric-ca:1.4        ...    ca.legal.toycorp.com
```
#### 4. Generate Fabric Peers docker containers

- `IMAGE_TAG`: See [hyperledger/fabric-peers](https://hub.docker.com/r/hyperledger/fabric-peer/tags) tags. 
- `COMPOSE_PROJECT_NAME`: prefix for your network

```bash
$ IMAGE_TAG=1.4 \
  COMPOSE_PROJECT_NAME=net \
  docker-compose -f docker-compose-peers.yaml up -d
```

Verify running Peer and Orderer containers
```
CONTAINER ID        IMAGE                            ...    NAMES
618017f0127b        hyperledger/fabric-orderer:1.4   ...    orderer.toycorp.com
c505b5a8f224        hyperledger/fabric-peer:1.4      ...    peer0.sales.toycorp.com
425acf83dcf2        hyperledger/fabric-peer:1.4      ...    peer0.accounting.toycorp.com
fc02a0e0525c        hyperledger/fabric-peer:1.4      ...    peer0.legal.toycorp.com
```

#### 5. Generate Fabric CLI docker containers

- `IMAGE_TAG`: See [hyperledger/fabric-tools](https://hub.docker.com/r/hyperledger/fabric-tools/tags) tags.
- `SYS_CHANNEL`: System Channel Name

```bash
$ IMAGE_TAG=1.4 \
  SYS_CHANNEL=sys.toycorp.channel \
  docker-compose -f docker-compose-cli.yaml up -d
```

Verify running CLI containers
```
CONTAINER ID        IMAGE                            ...    NAMES
ea7398aba66f        hyperledger/fabric-tools:1.4     ...    cli.legal.toycorp.com
8febaf865cd2        hyperledger/fabric-tools:1.4     ...    cli.accounting.toycorp.com
613491b4bad4        hyperledger/fabric-tools:1.4     ...    cli.sales.toycorp.com
```

## Other Commands
See [Fabric Commands](https://hyperledger-fabric.readthedocs.io/en/release-1.4/command_ref.html).
