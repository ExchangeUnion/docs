# Introduction

The Exchange Union Daemon ([`xud`](https://github.com/ExchangeUnion/xud)) is the reference node implementation powering [OpenDEX](https://opendex.network), a decentralized exchange (DEX) built on top of the [Lightning](https://lightning.network/) and [Raiden](https://raiden.network/) network. `xud` brings individual traders, market makers and exchanges onto OpenDEX to form a single global trading network and liquidity pool.

Traders use OpenDEX-enabled exchanges like [Boltz](https://boltz.exchange/) as gateways to trade on OpenDEX. These exchanges abstract away from the technological complexity of the DEX and offer a superior UX by enabling trading via a simple website. Exchanges use `xud` to hedge user trades on OpenDEX and lock in profits. Liquidity on OpenDEX is provided by market makers who use `xud` to take orders from external exchanges (like Binance) and offer these on OpenDEX adding a premium. Exchange Union provides SDK's for exchanges to automate hedging with `xud` and for market makers to connect to external exchanges and automate the arbitrage trades.

## Get Started

-> [Get started as **Trader**](User%20Guide.md), buying and selling cryptocurrency anonymously and account-free

-> [Get started as **Market Maker**](Market%20Maker%20Guide.md), providing liquidity from external exchanges making a profit

-> [Get started as **Exchange Operator**](), using the Boltz open-source white-label exchange platform to run your own exchange **(coming soon!)**

-> [Get started as **Developer**](Developer%20Guide.md), contributing or building on top of `xud`

![Trading via XUD](/images/orderbook.png)

## Incentives
* Traders benefit from peer-to-peer trading without KYC and accounts.
* Market makers make profits via arbitrage between external exchanges and OpenDEX.
* Exchanges make profits via locking in trading fees by hedging trades on OpenDEX.

## Features
* Supports individual traders, market makers and *and* exchanges
* Order book aggregates orders from the network locally.
* Orders get matched locally with peer orders from the network.
* Instant order settlement via atomic swaps on lightning/raiden.
* Complete control over private keys.
* One mnemonic for all assets.
* [Tor](https://www.torproject.org/) by default.
* Integration and simplified control of [lnd](https://github.com/lightningnetwork/lnd) and [raiden](https://github.com/raiden-network/raiden) clients.
* Peer-to-peer discovery of other OpenDEX nodes.
* gRPC API to serve other applications, also accessible via the command-line interface `xucli`.

## API & Code Documentation

Find the API documentation [here](https://api.exchangeunion.com) and code documentation [here](https://typedoc.exchangeunion.com/).


## Support & Community

* [Contribute](Contribute.md)
* Support, general questions and development-related discussions are welcome on our [Discord](https://discord.gg/YgDhMSn)

## Help us to improve!

`xud` is in an early stage; just like this page. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues) and the [docs](https://github.com/ExchangeUnion/docs/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!