# xucli

`xucli` is the command line interface that handles much of the basic interaction with a running `xud` instance. If `xud` is installed globally, it can be launched from any directory. In the recommended [xud-docker](User%20Guide.md) setup, it can be used from within the `xud ctl` shell.

To get a list of up-to-date commands, run:
```
$ xucli --help
xucli <command>

Commands:
  xucli addcurrency <currency>              add a currency
  <swap_client> [decimal_places]
  [token_address]
  xucli addpair <base_currency>             add a trading pair
  <quote_currency>
  xucli ban <node_pub_key>                  ban an xu node
  xucli channelbalance [currency]           get total channel balance for a
                                            given currency
  xucli connect <node_uri>                  connect to an xu node
  xucli create <password>                   create an xud node
  xucli discovernodes <peer_pub_key>        discover nodes from a specific peer
  xucli executeswap <pair_id> <order_id>    execute a swap on a peer order
  [quantity]                                (nomatching mode only)
  xucli getinfo                             get general info from the xud node
  xucli getnodeinfo <node_pub_key>          get general information about a node
  xucli listorders [pair_id]                list orders from the order book
  [include_own_orders] [limit]
  xucli listpairs                           get order book's available pairs
  xucli listpeers                           list connected peers
  xucli listtrades [limit]                  list completed trades
  xucli openchannel <nodePubKey>            open a payment channel
  <currency> <amount>
  xucli orderbook [pair_id] [precision]     display the order book, with orders
                                            aggregated per price point
  xucli buy <quantity> <pair_id> <price>    place a buy order
  [order_id] [stream]
  xucli sell <quantity> <pair_id> <price>   place a sell order
  [order_id] [stream]
  xucli removecurrency <currency>           remove a currency
  xucli removeorder <order_id> [quantity]   remove an order
  xucli removepair <pair_id>                remove a trading pair
  xucli shutdown                            gracefully shutdown the xud node
  xucli streamorders [existing]             stream order added, removed, and
                                            swapped events (DEMO)
  xucli unban <node_pub_key> [reconnect]    unban an xud node
  xucli unlock <password>                   unlock an xud node

Options:
  --help             Show help                                         [boolean]
  --version          Show version number                               [boolean]
  --rpcport, -p      RPC service port                   [number] [default: 8886]
  --rpchost, -h      RPC service hostname        [string] [default: "localhost"]
  --tlscertpath, -c  Path to the TLS certificate of xud                 [string]
  --json, -j         Display output in json format    [boolean] [default: false]
```

Calling any one of the commands above with the `--help` flag will output additional instructions for that particular command.

Examples for commands:
```bash
# Manually connect to another xud instance (has to be running the same network: simnet/testnet/mainnet)
$ xucli connect 02d8ac24fe2952d0d4c9eaa188a1d51ae4d80879fa311a3729a23a15b24f121d6c@xud1.testnet.exchangeunion.com:8885

# Places a new limit order BUYING 10 LTC for a price of 0.0079 BTC per LTC
$ xucli buy 10 LTC/BTC 0.0079

# Places a new limit order SELLING 2 LTC for the best market price
$ xucli sell 5 LTC/BTC market
```

## Help us to improve!

`xud` is in alpha stage, as well is this page. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues), [xud-simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and the [docs](https://github.com/ExchangeUnion/docs/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!