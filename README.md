# Setup your Hyperleder Chaincode Dev Environment

## You'll need...

### Git
```
$ git version
git version 2.10.1 (Apple Git-78)
```
### Go
```
$ go version
go version go1.7.3 darwin/amd64
```
Environment variables for GOTPATH, GOROOT need to be set
```
$ echo $GOROOT
/usr/local/go

$ echo $GOPATH
/Users/geahaad/dev/go
```
### Docker
```
$ docker -v
Docker version 1.13.0, build 49bf474

$ docker-compose -v
docker-compose version 1.10.0, build 4bd6f1a

$ docker-machine -v
```
Eventually you've to upgrade your existing docker image
```
$ docker-machine upgrade
Waiting for SSH to be available...
Detecting the provisioner...
Upgrading docker...
Stopping machine to do the upgrade...
Upgrading machine "default"...
Default Boot2Docker ISO is out-of-date, downloading the latest release...
Latest release for github.com/boot2docker/boot2docker is v1.13.0
Downloading /Users/geahaad/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v1.13.0/boot2docker.iso...
0%....10%....20%....3
```

### hyperledger-fabric
Grab the code from branch v0.6
```
# The v0.6 release exists as a branch inside the Gerrit fabric repository
git clone -b v0.6 http://gerrit.hyperledger.org/r/fabric
```
### Docker hyperledger/fabric
Run docker-compose to download those 3 docker images:
- hyperledger/fabric-membersrvc
- hyperledger/fabric-peer
- hyperledger/fabric-starter-kit

### Bluemix


## Let's get started

Start Docker
```
$ docker-machine start
Starting "default"...
[...]
```

Start Hyperledger Docker images
```
$ cd hyperleder-dev-setup
$ docker-compose up
Starting membersrvc
Starting peer
Starting starter
Attaching to membersrvc, peer, starter
```

## Deploy your first chaincode

Fork a copy of learn-chaincode from https://github.com/IBM-Blockchain/learn-chaincode

Change into sample code directory and register the chaincode via CLI
```
$ cd learn-chaincode/start
$ CORE_CHAINCODE_ID_NAME=myfirstchaincode CORE_PEER_ADDRESS=0.0.0.0:7051 ./start
```

### Login with a demo user (REST)
```
POST localhost:7050/registrar

{
  "enrollId": "jim",
  "enrollSecret": "6avZQLwcUe9b"
}
```

### Deploy (init) chaincode (REST)
```
{
  "jsonrpc": "2.0",
  "method": "deploy",
  "params": {
    "type": 1,
    "chaincodeID":{
        "name": "myfirstchaincode"
    },
    "CtorMsg": {
        "args":["init", "foobar"]
    },
    "secureContext": "jim"
  },
  "id": 1
}
```

### Invoke chaincode / Write (REST)
```
{
  "jsonrpc": "2.0",
  "method": "invoke",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"myfirstchaincode"
      },
      "CtorMsg": {
         "args":["write", "item1", "blob123"]
      },
      "secureContext": "jim"
  },
  "id": 3
}
```

### Query chaincode / Read (REST)
```
{
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"myfirstchaincode"
      },
      "CtorMsg": {
         "args":["read", "item1"]
      },
      "secureContext": "jim"
  },
  "id": 5
}
```
