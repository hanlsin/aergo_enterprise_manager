# Quickstart
This is for running Aergo Enterprise Manager using a few commands.

## Prepare Elasticsearch

```
cd ./aem/tools/elasticsearch
./start.sh
```

## Generate the config file

#### **`config/aem.conf`**
```
[network]
LISTEN_IP = 0.0.0.0
LISTEN_PORT = 5000

[database]
DATABASE_URI = sqlite:///$AEM_HOME/data/aem.db

[logging]
# [ERROR, INFO, DEBUG]
LOG_LEVEL = DEBUG
# Bytes
LOG_MAX_SIZE = 10485760
LOG_BACKUP_CNT = 5

[security]
CERT_FILE =
KEY_FILE =

[monitoring]
ES_HOST = 192.168.0.10
ES_PORT = 9200
```

You **MUST** input the external IP address for the `ES_HOST` value.

**Do not use `localhost` or `127.0.0.1`.**
Because `ES_HOST` is used by AEM and also a `Scan` container later.

## Set environment variable

```
cd ./aem/
export AEM_HOME=`pwd`
```

## Start AEM

```
cd ./aem/
./aem -u admin -p 1234
```

## Connect AEM

Go to http://localhost:5000 and login with the username and password which are used when to start AEM.

## New Project

* Project Name: `IsDB SCMS`

Save and jump into the project.

## Node Image

You need to register a node image.

### Extract Node Image (terminal)

Especially, for IsDB, you need to use `hanlsin/IsDB` for the configurable receipt size.

```
docker pull hanlsin/aergo.isdb:latest
docker save hanlsin/aergo.isdb:latest > aergo.isdb-latest.tar
```

### New Image (AEM web)

* Select File: `aergo.isdb-latest.tar`

Save after calculating `Image Hash`.

## Blockchain Nodes > New Host

Please use a docker daemon (engine) host address.

* Host Name: `local`
* Host IP: `localhost`
* Host Port: `2375`

For Mac, you need to use the `socat` utility to connect the `docker desktop`.

```
brew install socat
socat TCP-LISTEN:2375,reuseaddr,fork UNIX-CONNECT:/var/run/docker.sock
```

The `docker desktop` doesn't support external TCP engine APIs.

## Blockchain Nodes > New Node

* Host: `local`
* Chain ID: `isdb scms testnet1`
* Node Name: `local node 1`

**NEXT**

* Node Image: `aergo.isdb-latest.tar / X`
* Node Type: `bp`
* Block Generation Rate: `1` (it means 1 second)
* RPC Port: `7845`
* P2P Port: `7846`

If you want to add more nodes in the same host and for the same blockchain, you need to use different `RPC Port` and `P2P Port`; for example, `7855` and `7856`.

**NEXT**

* Log Level: `debug`
* Administrator: *ignore at this moment*

**SAVE**

### Checking docker containers (terminal)

Now you can check containers like this:
```
$ docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
48fbc0e3dd8b        870ed3150136                                          "/usr/local/bin/dock…"   5 seconds ago       Up 4 seconds                                                         aergometric-127.0.0.1
50bbb983e9b5        f06d7fe42bde                                          "fluent-bit -c aergo…"   2 minutes ago       Up 2 minutes        2020/tcp                                         aergostat-05cb5a19-a681-4671-80e2-d3043239240b
446cc5b96c10        0227bd6e3f27                                          "/bin/sh -c 'aergosv…"   2 minutes ago       Up 2 minutes        0.0.0.0:7845-7846->7845-7846/tcp                 aergonode-05cb5a19-a681-4671-80e2-d3043239240b
2d78de38f2b4        docker.elastic.co/elasticsearch/elasticsearch:6.7.0   "/usr/local/bin/dock…"   2 hours ago         Up 2 hours          0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   elasticsearch
```

## Admin --> Monitoring

Now you can see the stats of `Host`, `Blockchain` and `Node`.

## Monitoring --> Scan

### New Instance

* Instance Name: `Scan IsDB Local Node 1`
* Node: `local node 1`

**SAVE**

### Select Instance

Now you can find all `blockchain info`, `blocks' data`, `transactions`, and `accounts` like `Aergo Scan`.
