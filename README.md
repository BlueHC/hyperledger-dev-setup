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
