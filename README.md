# ethereum-docker

Local Ethereum development and transaction tests need nodes and funded accounts. Use Docker to start a single geth test node or a multi-node cluster with a Netstats dashboard.

## Features

- Run a standalone Ethereum test node with the Go Ethereum client (geth).
- Start a cluster with a bootstrap node, a connected Ethereum node, and a Netstats dashboard.
- Scale the cluster's `eth` service and connect additional nodes through the bootstrap node.
- Create 10 test accounts with 20 Ether each through the bootstrap configuration.
- Attach to a node's geth JavaScript console to inspect its state or start mining.

## Installation

Install Docker and Docker Compose, then download or check out this repository. From the repository directory, verify that the tools are available:

```sh
cd ethereum-docker
docker --version
docker-compose --version
```

The commands below use the legacy `docker-compose` CLI. These configurations also use legacy geth interfaces and mining commands; compatibility with current geth releases is not established here.

For an existing Docker Toolbox setup, create a machine named `default` if needed and configure your shell to use it:

```sh
docker-machine create --driver virtualbox default
eval "$(docker-machine env default)"
```

Skip this step if Docker already runs locally.

## Usage

### Start a single test node

```sh
docker-compose -f docker-compose-standalone.yml up -d
```

The JSON-RPC endpoint is on port `8545` of the Docker host. With a local Docker daemon, use `http://localhost:8545`. With Docker Toolbox, obtain the host address with:

```sh
docker-machine ip default
```

**Use these nodes only for development and tests.** The supplied geth setup enables all APIs by default. Keep its endpoints away from untrusted networks, and do not use the test accounts for real funds.

### Start a cluster

```sh
docker-compose up -d
```

This starts a bootstrap container, an Ethereum node that connects to it, and a Netstats container. Open `http://localhost:3000` for the dashboard when Docker runs locally. For Docker Toolbox:

```sh
open http://$(docker-machine ip default):3000
```

To increase the number of `eth` service containers to three:

```sh
docker-compose scale eth=3
```

The additional nodes connect through the bootstrap node. To change the initial account balances, edit `files/genesis.json` before bootstrapping the chain. The allocated Ether is for this test chain.

### Attach to geth

For the default Compose container name:

```sh
docker exec -it ethereumdocker_eth_1 geth attach ipc://root/.ethereum/devchain/geth.ipc
```

If your Compose project produces a different container name, find it with `docker-compose ps` and substitute it in the command. At the JavaScript console, start mining and inspect its status:

```js
miner.start()
web3.eth.mining
```

### Stop the containers

For the cluster:

```sh
docker-compose down
```

For the standalone node:

```sh
docker-compose -f docker-compose-standalone.yml down
```

<!-- seorepo:network:start -->
## More tools from our network

- [**octo-cli**](https://github.com/boostlyapi/octo-cli) — CLI на Go для публикации данных из PostgreSQL, MSSQL и MySQL через бессерверные веб-сервисы в OpenFaaS или Knative. Конфигурация сервиса задаётся в YAML.
- [**tool**](https://github.com/boostlyapi/tool) — CLI на Go для Apache Kafka: чтение и отправка сообщений в JSON, просмотр топиков, управление смещениями consumer groups и базовое администрирование.
- [**docker-ikev2-vpn-server**](https://github.com/Cabral-it/docker-ikev2-vpn-server) — Docker-based IKEv2 VPN server with shared-secret authentication and .mobileconfig generation for iOS and macOS, based on gaomd/docker-ikev2-vpn-server.
- [**akrobateo**](https://github.com/Dyooooou/akrobateo) — A Go Kubernetes operator that exposes LoadBalancer services through node hostPorts using DaemonSets and synchronizes their load balancer addresses.
- [**node-hnsn**](https://github.com/ghb13/node-hnsn) — A TypeScript node for decentralized document timestamping with Bitcoin and IPFS, with an HTTP API for signed work claims and file uploads.
- [**wg-access-server**](https://github.com/EngHirsi/wg-access-server) — A WireGuard VPN server with a web UI for user authentication, one-click device registration, QR-code setup, and configurable network isolation.
<!-- seorepo:network:end -->

## License

MIT. This project is based on [AIE-London/ethereum-docker](https://github.com/AIE-London/ethereum-docker), which is distributed under the MIT license. Retain the original copyright and license notices when redistributing its code.
