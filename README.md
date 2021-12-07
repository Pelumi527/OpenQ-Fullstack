# OpenQ-Fullstack

Welcome to OpenQ! The world's simplest crypto-powered bounty system.

Let's get you started.

## Step 1: Cloning all Repositories

The OpenQ fullstack consists of 5 containerized microservices and a local JSON RPC Ethereum node.

Let's clone the repositories for them now.

To clone all repositories, run:

```bash
./boot.sh 
```

## Step 2: Setting up .env files

You'll see an error:

`NOTE: You will need .env files in the root of the following repositories:`

Let's add .env files to each repository now.

Follow the below very closely.

### OpenQ-Frontend .env
1. Create a file called `.env` in the root of the `OpenQ-Frotnend` project.

Then add your Github Personal Access Token (PAT):

You can get a PAT [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Necessary Scopes:
`admin:org`
`repo`
`user`

```bash
PAT=<YOUR PAT HERE>
OPENQ_ID=5fbd39c6916b7efb63cc
OPENQ_SUBGRAPH_HTTP_URL=http://localhost:8000/subgraphs/name/openqdev/openq
OPENQ_SUBGRAPH_WEBSOCKET_URL=ws://localhost:8001/subgraphs/name/openqdev/openq
```

#### OpenQ-Frontend .env for Running without The Graph locally

1. Checkout `mumbai` branch on `OpenQ-Fullstack` and `OpenQ-Frontend`

2. In `OpenQ-Frontend/.env`:

```bash
PAT=<YOUR PAT HERE>
OPENQ_ID=5fbd39c6916b7efb63cc
OPENQ_SUBGRAPH_HTTP_URL=https://api.thegraph.com/subgraphs/name/openqdev/openq-development
OPENQ_SUBGRAPH_WEBSOCKET_URL=wss://api.thegraph.com/subgraphs/name/openqdev/openq-development
```

In `OpenQ-Contracts/.env.contracts`:

```bash
OPENQ_ADDRESS="0x05c51563Fa3F3088f239060Fd782cb60D083281e"
FAKE_TOKEN_ADDRESS="0x8d57166B3093c7E1631Bb1A248A8BA2B76Aea890"
MOCK_TOKEN_ADDRESS="0x6075040CdA6E5C12483295D7287C8c44aBb0F975"
```

In `OpenQ-Contracts/.env`:

```bash
PROVIDER_URL=https://rpc-mumbai.maticvigil.com/v1/258e87c299409a354a268f96a06f9e6ae7ab8cea
WALLET_KEY=<A PRIVATE KEY WITH SOME MUMBAI ON IT>
CHAIN_ID=80001
BLOCK_EXPLORER_BASE_URL="https://mumbai.polygonscan.com"
```

3. Then run `./boot.sh`

4. Add MOCK and FAKE to your Mumbai Metamask wallet so you can use them for funding and checking refunds.

5. YOU WILL HAVE ERRORS ONCE YOU CLICK ON AN ORGANIZATION! It comes from the fact that I've hardcoded a contract mapping since of course Mock Token and Fake don't
exist in real life, and there's no contract mapping for Mumbai. This is part of the updates we need. Check out `useGetTokenValues` to edit it to make it work yet again.

### OpenQ-Contracts .env

Create a file called `.env` in the root of the `OpenQ-Github-OAuth-Server` project.

```bash
PROVIDER_URL=http://ethnode:8545
WALLET_KEY=<WALLET_KEY>
CHAIN_ID=31337
BLOCK_EXPLORER_BASE_URL="https://mumbai.polygonscan.com" # This doesnt actually matter when running locally
```

### OpenQ-Github-OAuth-Server .env
Create a file called `.env` in the root of the `OpenQ-Github-OAuth-Server` project.

```bash
OPENQ_ID=5fbd39c6916b7efb63cc
OPENQ_SECRET=<get from admin>
ORIGIN_URL=http://localhost:3000
```

Contact @FlacoJones for the development OAuth client secret.

### OpenQ-CoinAPI .env
Create a file called `.env` in the root of the `OpenQ-CoinAPI` project.

In Docker-Compose:
```bash
REDIS_URL=redis
ORIGIN_URL=http://localhost:3000
```

### OpenQ-API .env
Create a file called `.env` in the root of the `OpenQ-API` project.

```bash
DATABASE_CONNECTION_STRING="mongodb+srv://admin:<password>!@openq-mongo.y8tho.mongodb.net/user?retryWrites=true&w=majority"
```

### OpenQ-Oracle .env
Create a file called `.env` in the root of the `OpenQ-Oracle` project.

```bash
ORIGIN_URL="http://localhost:3000"
```

### Step 3: Booting Fullstack with docker-compose

For local development, we orchestrate with [docker-compose](https://docs.docker.com/compose/).

For development.openq.dev, staging.openq.dev, and app.openq.dev (production) we orchestrate with [Kubernetes](https://kubernetes.io/).

Now that :

```bash
./boot.sh 
```

## Access Services

| OpenQ Service      | URL |
| ----------- | ----------- |
| OpenQ-Frontend      | localhost:3000       |
| OpenQ-API      | localhost:4000       |
| OpenQ-Oracle   | localhost:8090        |
| OpenQ-Github-OAuth-Server   | localhost:3001        |
| OpenQ-CoinAPI   | localhost:8081        |
| Redis   | localhost:6379       |
| OpenQ-JSON-RPC-Node   | localhost:8545       |


Any changes you make to an application will trigger an update.

Run with sudo in case your having trouble with the bash script / docker compose only works in old versions

## Mumbai Contract Addresses

OPENQ_ADDRESS: 0x0c3dCa7865203A9bbdC83942a3f1B1567D331Aa6

MOCK_TOKEN_ADDRESS: 0x6444FB48C0c640e6C25150bb17b9E9d842126043

FAKE_TOKEN_ADDRESS: 0xA1c3A3dBFcF4E0BE3f023BAB58254fB4CcB10127

## Clear Images and Rebuild when Adding Dependencies

Clear cache and rebuild images after changing dependencies by running the following commands:

```bash
docker system prune
```

### Stop and Remove All Containers

```bash
docker stop $(docker ps -aq) && docker rm $(docker ps -aq)
```

### Remove all images

```bash
docker rmi -f $(docker images -a -q)
```

### Remove All Volumes

```bash
docker volume rm $(docker volume ls -q)
```

### Kill Process on a Port

```bash
lsof -ti tcp:<PORT> | xargs kill
```

```bash
./boot.sh --build
```