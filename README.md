<h1 align="center">zkSync Era Block Explorer</h1>

<p align="center">Online blockchain browser for viewing and analyzing <a href="https://zksync.io">zkSync Era</a> blockchain.</p>

## 📌 Overview
This repository is a monorepo consisting of 3 packages:
- [Worker](./packages/worker) - an indexer service for [zkSync Era](https://zksync.io) blockchain data. The purpose of the service is to read the data from the blockchain in real time, transform it and fill in it's database with the data in a way that makes it easy to be queried by the [API](./packegs/api) service.
- [API](./packages/api) - a service providing Web API for retrieving structured [zkSync Era](https://zksync.io) blockchain data collected by [Worker](./packages/worker). It connects to the Worker's database to be able to query the collected data.
- [App](./packages/app) - a front-end app providing an easy-to-use interface for users to view and inspect transactions, blocks, contracts and more. It makes requests to the [API](./packages/api) to get the data and presents it in a way that's easy to read and understand.

## 🏛 Architecture
The following diagram illustrates how are the block explorer components connected:

```mermaid
flowchart
  subgraph blockchain[Blockchain]
    Blockchain[zkSync Era JSON-RPC API]
  end

  subgraph explorer[Block explorer]
    Database[("Block explorer DB<br/>(PostgreSQL)")]
    Worker(Worker service)
    API(API service)
    App(App)

    Worker-.Save processed data.->Database
    API-.Query data.->Database
    App-."Request data (HTTP)".->API
    App-."Request data (HTTP)".->Blockchain
  end

  Worker-."Request data (HTTP)".->Blockchain
```

[Worker](./packages/worker) service is responsible for getting data from blockchain using [zkSync Era JSON-RPC API](https://era.zksync.io/docs/api/api.html), processing it and saving into the database. [API](./packages/api) service is connected to the same database where it gets the data from to handle API requests. It performs only read requests to the database. The front-end [App](./packages/app) makes HTTP calls to the Block Explorer [API](./packages/api) to get blockchain data and to the [zkSync Era JSON-RPC API](https://era.zksync.io/docs/api/api.html) for reading contracts, performing transactions etc.

## 🚀 Features

- ✅ View transactions, blocks, transfers and logs.
- ✅ Inspect accounts, contracts, tokens and balances.
- ✅ Verify smart contracts.
- ✅ Interact with smart contracts.
- ✅ Standalone HTTP API.
- ✅ Local node support.

## 📋 Prerequisites

- Ensure you have `node >= 18.0.0` and `npm >= 9.0.0` installed.

## 🛠 Installation

```bash
$ npm install
```

## ⚙️ Setting up env variables

Make sure you have set up all the necessary env variables. Follow [Setting up env variables for Worker](./packages/worker#setting-up-env-variables) and [Setting up env variables for API](./packages/api#setting-up-env-variables) for instructions.

## 👨‍💻 Running locally

Before running the solution, make sure you have a database server up and running and you have created a database.

To run all the components (`Worker`, `API` and front-end `App`) in `development` mode run the following command from the root directory.
```bash
$ npm run dev
```

For `production` mode run:
```bash
$ npm run build
$ npm run start
```
To verify front-end `App` is running open http://localhost:3010 in your browser. `API` should be available at http://localhost:3000. `Worker` - http://localhost:3001.

Each component can also be started individually. Follow individual packages `README` for details.

## 🕵️‍♂️ Testing
Run unit tests for all packages:
```bash
$ npm run test
```
Run e2e tests for all packages:
```bash
$ npm run test:e2e
```
Run tests for a specific package:
```bash
$ npm run test -w {package}
```
For more details on testing please check individual packages `README`.

## 💻 Conventional Commits
We follow the Conventional Commits [specification](https://www.conventionalcommits.org/en/v1.0.0/#specification).

## 📘 License
MIT License.
