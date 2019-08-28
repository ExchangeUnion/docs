# Exchange Guide

This guide describes how to integrate `xud` into a digital asset exchange platform and how to test its trading functionalities with the `xud-simnet`.

## Disclaimer
WIP! `xud` for exchanges is in early alpha stage and this guide aims to give exchange operators a "feel" of how things will work. `xud` in its current stage should not be connected to a production system or be configured for mainnet usage. The `xud-simnet` uses test coins at all times. We are working on an improved setup using docker, which will also be the preferred way to use `xud` for exchanges in production. We recommend integrating `xud` with a [demo exchange setup](https://github.com/ExchangeUnion/xud-exchange-integration-example) first, to get a look and feel for `xud`'s API before connecting `xud` to an existing exchange test environment.

### XUD SimNet

Follow [this guide](SimNet-Guide) to set up the XUD SimNet. Once the setup of `xud-simnet` is completed, you will be able to query pending orders, place orders, and experiment with `xud`'s rich set of commands. This guide is written for Linux & MacOS operating systems and geared towards IT administrators of exchanges.


### API Integration

`xud` uses [gRPC](https://grpc.io/). The latest overview of available calls with descriptions can be found in [this proto file](https://github.com/ExchangeUnion/xud/blob/master/proto/xudrpc.proto). Once the gRPC is more stable, we'll provide a nicer looking documentation.

From a high-level perspective, `xud` supports two modes: `matching` and `nomatching`

Default is `matching` mode, which means that `xud` acts as order book & matching engine for all of your orders. Choose this mode if your exchange is not live yet, comparably small or you simply want to rather rely on the matching engine of `xud` instead of using your own. You will need to design your exchange in a way to add orders to `xud`'s order book and listen to order events, in order to display real-time order information to your users and update user account balances. The following API calls are relevant:
* `SubscribeAddedOrders` this streaming call subscribes to orders being added to the order book. Together with `SubscribeRemovedOrders`, this allows the client to maintain an up-to-date view of the order book. Use this call to show a real-time order book to users via your exchange front-end and update account balances for your users.
* `SubscribeRemovedOrders` this streaming call subscribes to orders being  removed - either in full or in part - from the order book. Together with `SubscribeAddedOrders`, this allows the client to maintain an up-to-date view of the order book. Use this call to show a real-time order book to users via your exchange front-end and update account balances for your users.
* `PlaceOrder` use this to add an order to `xud`'s order book, which was triggered by a user of your platform. You can choose the sync version of the call, which returns the final result. This can take some seconds if the order gets swapped with a remote peer. The async version updates with events in the course of the swap via `SubscribeSwaps`. Currently buy/sell as market/limit order are supported. 
* `RemoveOrder` removes an order previously placed via `PlaceOrder`, which is triggered by a user of your platform.
* `SubscribeSwaps` informs about successful/failed swaps with other peers in an event stream. Use this call to track order executions, update balances, and inform users when one of their orders was settled.

`nomatching` mode, which can be enabled via [xud.conf](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf), is designed for all order matching being handled by your existing matching engine and `xud` mainly a) informs about new & removed orders from other peers and b) executes swaps with peers when a match in your matching engine was found. The following API calls are relevant:
* `SubscribeAddedOrders` informs about new `peerOrder`s from other peers in an event stream. Use this call to include these `peerOrder`s in your internal order book & matching engine. Adjust your system to call `ExecuteSwap`, if a match is found for a specific `peerOrder`.
* `SubscribeRemovedOrders` informs about `peerOrder`s being removed - either in full or in part - in an event stream. Use this call to remove these `peerOrder`s from your internal order book & matching engine. These orders were invalidated by the peer and are no longer available for trading.
* `PlaceOrder` broadcasts your orders to other peers. This call is async in `nomatching` mode and you'll get updates of peers swapping your orders via `SubscribeSwaps`. Currently buy/sell as market/limit order are supported.
* `SubscribeSwaps` informs about successful/failed swaps with other peers in an event stream and is to be used with `PlaceOrder`. Use this call to get real-time swap events from placed orders to track order executions, update your user's balances, and inform users when one of their orders was settled.
* `ExecuteSwap` executes a swap for an match your internal matching engine found with a remote peer. This call requires `orderID`, `pairID` & `quantity`. You can continue using your internal `orderID`s, `xud` takes care of mapping such with `orderID`s in the Exchange Union network. This is a sync call, returning the final result of a swap. It might take several seconds for the swap to complete.

### Notes
* `xud` doesn't *forward* orders from one peer to another; it only sends to and receives from direct peers. The main reasons for this, among others, are speed and prevention of manipulation. A more scalable approach will be [introduced in a later stage](https://github.com/ExchangeUnion/Docs/blob/master/XU-TechnicalPaper.md#xus-dob-protocol-has-the-following-goals).
* Restarting or stopping `xud` clears all orders from its order book. When `xud` boots up, it automatically reconnects to its peers and fetches all open orders.
* `xud` relies on is its own (local) view of orders. This is also how it determines if it acts as a maker or a taker when a match with a peer order is found. If the peer order reached the order book first it acts as taker, if the local order reached the order book first it acts as maker. The swap protocol defines that only as taker, `xud` actively initiates a swap, as maker it'll wait for the swap to be initiated by the remote taker. For the same reason, `xud` never matches incoming peer orders with the existing order book; only incoming local orders. Due to network latency it can now happen that both parties determine themselves to be the maker when they place their orders nearly simultaneously. In this case, a swap won't be initiated - nothing happens.
* To query existing orders one can use `ListOrders`, in addition to the `existing` flag for the `SubscribeAddedOrders` streaming call.
* Peer discovery and auto-connecting to new peers is still experimental and only enabled gradually. In order to manually connect to a peer via CLI, use `connect [NodeURI]` where `NodeURI` is `pubkey@IP:Port`, e.g. `02b66438730d1fcdf4a4ae5d3d73e847a272f160fee2938e132b52cab0a0d9cfc6@xud1.test.exchangeunion.com:8885`.
* The gRPC API is locally exposed on port 8886 by default.
* `nomatching` mode introduces the problem of peer orders racing against local orders since most exchange's matching engines don't support the necessary `hold` mechanism. Several approaches are currently being evaluated and tested, follow the status [here](https://github.com/ExchangeUnion/xud/issues/587).

### Help us to improve!

`xud` is in alpha stage, as well as this guide. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), the [simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and this [wiki](https://github.com/ExchangeUnion/xud-wiki/issues).

Feel like talking? Chat with us on [Gitter](https://gitter.im/exchangeunion/xud-testing)!