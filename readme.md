# sifnode

**sifnode** is a blockchain application built using Cosmos SDK and Tendermint and generated with [Starport](https://github.com/tendermint/starport).

## Requirements

- [Ruby 2.6.x](https://www.ruby-lang.org/en/documentation/installation)
- [Golang](https://golang.org/doc/install)
- [Docker](https://docs.docker.com/get-docker)

## Getting started

### Setup

1. Ensure you've installed the dependencies listed above.

2. Clone the repository:

```
git clone ssh://git@github.com/Sifchain/sifnode && cd sifnode
```

3. Checkout the latest testnet release:

```
git checkout tags/monkey-bars-testnet-3
```

4. Build:

```
make install
```

5. If you're a new operator (and only if - as otherwise this will reset your node!): 

    5.1 Change to the `build` directory:

    ```
    cd ./build
    ```

    5.2 Scaffold your new node:
    
    ```
    rake 'genesis:sifnode:scaffold[monkey-bars, bd17ce50e4e07b5a7ffc661ed8156ac8096f57ce@35.166.247.98:26656, http://35.166.247.98:26657/genesis]'
    ```

6. If you're an existing node operator:

    6.1 Reset your node state:
    
    ```
    sifnoded unsafe-reset-all
    ```

    6.2 Get the latest genesis file:

    ```
    wget -O ~/.sifnoded/config/genesis.json https://raw.githubusercontent.com/Sifchain/networks/feature/genesis/testnet/monkey-bars-testnet-3/genesis.json
    ```
   
    6.3 Update your persistent peers in the file `~/.sifnoded/config/config.toml` so that it reads: 

    ```
    persistent_peers = "bd17ce50e4e07b5a7ffc661ed8156ac8096f57ce@35.166.247.98:26656,f8f5d01fdc73e1b536084bbe42d0a81479f882b3@35.166.247.98:28002,f27548f03a4179b7a4dc3c8a62fcfc5f84be15ff@35.166.247.98:28004,dd35505768be507af3c76f5a4ecdb272537e398f@35.166.247.98:28006"
    ```

7. Start your node:

```
sifnoded start
```

and within a few seconds, your node should start synchronizing with the network.

You can verify that you're connected by running:

```
sifnodecli q tendermint-validator-set
```

## Becoming a validator

The instructions above will get you connected to the network, but you won't be able to participate in consensus until you become a validator.

Before you become a validator, please reach out to us on [Discord](https://discord.com/invite/zZTYnNG) to request some tokens.

You will also require the moniker of your node, when becoming a validator. You can find this by running:

```
cat ~/.sifnoded/config/config.toml| grep moniker
```

Once you have received your tokens, you can become a validator simply by running:

```
sifnodecli tx staking create-validator \
    --commission-max-change-rate 0.1 \
    --commission-max-rate 0.1 \
    --commission-rate 0.1 \
    --amount 1000000000trwn \
    --pubkey $(sifnoded tendermint show-validator) \
    --moniker <moniker> \
    --chain-id monkey-bars \
    --min-self-delegation 1 \
    --gas auto \
    --from <moniker> \
    --keyring-backend file
```

*Please replace `<moniker>` with the moniker of your node.*

## Peers

New node operators may also use the following peer addresses:

```
f8f5d01fdc73e1b536084bbe42d0a81479f882b3@35.166.247.98:28002
f27548f03a4179b7a4dc3c8a62fcfc5f84be15ff@35.166.247.98:28004
dd35505768be507af3c76f5a4ecdb272537e398f@35.166.247.98:28006
```

## Additional Resources

- [Additional instructions on standing up Sifnode](https://www.youtube.com/watch?v=1kjdjCEcYak&feature=youtu.be&ab_channel=utx0_).
- [Instructions on using Ethereum <> Sifchain cross-chain functionality](https://youtu.be/r81NQLxMers).

You can also ask questions on Discord [here](https://discord.com/invite/zZTYnNG).
