# Introduction

The Exchange Union Daemon ([`xud`](https://github.com/ExchangeUnion/xud)) is the open-source node software powering [Exchange Union](https://www.exchangeunion.com/), a decentralized exchange (DEX) built on the [Lightning](https://lightning.network/) and [Raiden](https://raiden.network/) network.

`xud` brings individuals and exchanges onto the same network to form a single global liquidity pool. This gives users a choice to either trade directly on the DEX by running `xud`, managing private keys and software stack, *or* to conveniently trade via a trusted exchange. Exchanges benefit from access to the network's aggregated liquidity and can provide deeper order books and new trading pairs to their users.

## Get Started

-> [Get started as **User**](User%20Guide.md), buying and selling cryptocurrency

-> [Get started as **Exchange**](Exchange%20Guide.md), tapping into the network's liquidity

-> [Get started as **Developer**](Developer%20Guide.md), contributing or building on top of `xud`

![Trading via XUD](/images/orderbook.png)

## Features
* No central point or authority.
* No middleman.
* No KYC.
* Supports individual traders *and* exchanges.
* Market makers earn fees.
* Direct, peer-to-peer trading.
* Order book aggregates orders from the network locally.
* Orders get matched locally with peer orders from the network.
* Built-in arbitrage. Get the best price.
* Instant order settlement via atomic swaps on lightning/raiden.
* User has complete control over private keys.
* One mnemonic for all assets.
* [Tor](https://www.torproject.org/) by default.
* Integration and simplified control of [lnd](https://github.com/lightningnetwork/lnd) and [raiden](https://github.com/raiden-network/raiden) clients.
* Peer-to-peer discovery of other nodes.
* gRPC API to serve other applications, also accessible via the command-line interface `xucli`.

## API & Code Documentation

Find the API documentation [here](https://api.exchangeunion.com) and code documentation [here](https://typedoc.exchangeunion.com/).


## Support & Community

* [Contribute](Contribute.md)
* Support, general questions and development-related discussions are welcome on our [Discord](https://discord.gg/YgDhMSn)

## Help us to improve!

`xud` is in alpha stage, as well is this page. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues), [xud-simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and the [docs](https://github.com/ExchangeUnion/docs/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!