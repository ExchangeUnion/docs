The Exchange Union Daemon ([`xud`](https://github.com/ExchangeUnion/xud)) is the reference implementation powering [OpenDEX](https://opendex.network), a decentralized exchange built on top of the [Lightning](https://lightning.network/) and [Raiden](https://raiden.network/) network. `xud` brings individual traders, market makers and exchanges onto OpenDEX to form a single global trading network and liquidity pool.

**Traders** use [OpenDEX-enabled exchanges](https://opendex.network/trade/exchanges) as gateways to trade on OpenDEX. These exchanges are usually non-custodial and are operated by different entities. Under the hood, they interact with the OpenDEX network and abstract the complexity that comes with it into a nice user interface for traders. **Exchanges** use `xud` to hedge user trades using the liquidity on OpenDEX in order to lock in profits. **Market Makers** use `xud` to offer liquidity on OpenDEX leveraging arbitrage to external exchanges, like Binance. Exchange Union provides tools that help decrease the friction for both sides of the marketplace. Exchanges use `hedgy`, a tool to automate the hedging with `xud` and market makers use `arby`, a tool that automates leveraging arbitrage opportunities between OpenDEX and external exchanges.

## Get Started

-> [Get started as **Market Maker**](Market%20Maker%20Guide.md), providing liquidity from external exchanges making a profit

-> [Get started as **Exchange Operator**](), running a open-source exchange platform with integrated liquidity **(coming soon!)**

-> [Get started as **Trader**](User%20Guide.md), buying and selling cryptocurrency preserving your privacy & without counterparty risk

-> [Get started as **Developer**](Developer%20Guide.md), contributing or building on top of `xud`

![Trading via XUD](/images/orderbook.png)

## Incentives
* Traders benefit from instant & anonymous peer-to-peer trading without KYC and accounts.
* Market makers make profits via arbitrage between external exchanges and OpenDEX.
* Exchanges make profits via locking in trading fees by hedging trades on OpenDEX.

## Features
* Supports individual traders, market makers, *and* exchanges.
* Order book aggregates orders from the network locally.
* Orders get matched locally with peer orders from the network.
* Instant order settlement via atomic swaps on the lightning & raiden network.
* Full control over assets.
* One mnemonic for all assets.
* [Tor](https://www.torproject.org/) by default.
* Integration and simplified control of [lnd](https://github.com/lightningnetwork/lnd) and [raiden](https://github.com/raiden-network/raiden) clients.
* Peer-to-peer discovery of other OpenDEX nodes.
* gRPC API to serve other applications, also accessible via the command-line interface `xucli`.

## API & Code Documentation

The daemon has been designed to be as developer friendly as possible in order to facilitate application development on top of `xud`.
* [api.exchangeunion.com](https://api.exchangeunion.com): The automatically generated gRPC API documentation
* [typedoc.exchangeunion.com](https://typedoc.exchangeunion.com/): The automatically generated code documentation


## Support & Community

* [Contribute](Contribute.md)!
* Support, general questions and development-related discussions are welcome on our [Discord](https://discord.gg/YgDhMSn)!

## Help us to improve!

`xud` is in an early stage; just like this page. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues) and the [docs](https://github.com/ExchangeUnion/docs/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!   