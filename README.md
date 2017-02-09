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

## Deploy your first chaincode to your local docker hyperledger fabric

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
POST localhost:7050/chaincode

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
POST localhost:7050/chaincode

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
GET localhost:7050/chaincode

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

## Deploy your first chaincode to Bluemix

- Add the Blockchain service to your Bluemix account: https://console.eu-gb.bluemix.net/catalog/services/blockchain/
- Open the Blockchain Dashboard
- Copy one of the peer URLs and use it as PEER_URL in the upcoming snippets
- Check the Service Credentials at the bottom right on the Network page and copy credentials from one user, use it as MY_CREDENTIALS

### Login with a demo user (REST)
```
POST https://[PEER_URL]/registrar

{
  "enrollId": "user_type1_2",
  "enrollSecret": "[MY_CREDENTIALS]"
}
```

### Deploy (init) chaincode (REST)
```
POST https://[PEER_URL]/chaincode

{
     "jsonrpc": "2.0",
     "method": "deploy",
     "params": {
         "type": 1,
         "chaincodeID": {
           "path": "https://github.com/BlueHC/hyperledger-learn-chaincode/finished"
         },
         "ctorMsg": {
             "function": "init",
             "args": [
                 "Welcome to our Hyperledger Genesis Session @ BlueHC!"
             ]
         },
         "secureContext": "user_type1_2"
     },
     "id": 5
 }
```

Remember chaincode id from response for placeholder CC_ID.

### Invoke chaincode / Write (REST)
```
POST https://[PEER_URL]/chaincode

{
  "jsonrpc": "2.0",
  "method": "invoke",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"[CC_ID]"
      },
      "CtorMsg": {
      	"args":["write", "item0", "blob123"]
      },
      "secureContext": "user_type1_2"
  },
  "id": 1
}
```

### Query chaincode / Read (REST)
```
GET https://[PEER_URL]/chaincode

{
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"[CC_ID]"
      },
      "CtorMsg": {
      	"args":["read", "hello_world"]
      },
      "secureContext": "user_type1_2"
  },
  "id": 1
}
```
