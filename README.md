# ![XUD](images/xud-logo.png "XUD Logo") Documentation

## Introduction

XUD, the Exchange Union Daemon, is the open-source node software powering [Exchange Union](https://www.exchangeunion.com/), a decentralized exchange (DEX) built on the [Lightning](https://lightning.network/) and [Raiden](https://raiden.network/) network.

The vision is to bring individuals and exchanges onto the same network and form one global liquidity pool. This gives users a choice to either trade directly on the DEX by running `xud`, managing private keys and software stack, *or* to conveniently trade via a trusted exchange. Exchanges benefit from access to the network's aggregated liquidity and can provide deeper order books and new trading pairs to their users.

-> [Get started as User, buying and selling crypto](user-guide.md) <-

-> [Get started as Exchange, tapping into the network's liquidity](exchange-guide.md) <-

-> [Get started as Developer, building applications on top of xud](developer-guide.mde) <-

![xud orderbook](/images/orderbook.png)

## Features:
* No central point or authority.
* No middleman.
* No KYC.
* Supports individual traders *and* exchanges.
* Market makers earn fees.
* Direct, peer-to-peer trading.
* Order book aggregates orders from the network locally.
* Orders get matched locally with peer orders.
* Instant order settlement via atomic swaps on lightning/raiden.
* User has complete control over private keys.
* One mnemonic for all assets.
* [Tor](https://www.torproject.org/) by default.
* Integration and simplified control of [lnd](https://github.com/lightningnetwork/lnd) and [raiden](https://github.com/raiden-network/raiden) clients.
* Peer-to-peer discovery of other nodes.
* gRPC API to serve other applications, also accessible via the command-line interface `xucli`.

## API & Code Documentation

Read the API documentation [here](http://api.exchangeunion.com) and code documentation [here](http://typedoc.exchangeunion.com/).


## Support & Community

* [Contribute](CONTRIBUTE.md)
* Questions and development-related discussions on [Discord](https://discord.gg/YgDhMSn)