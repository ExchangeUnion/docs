We recommend to use the [xud-docker](https://github.com/ExchangeUnion/xud/wiki/Docker) setup to trade on the `xud-simnet`; it does not require installation of `xud` or any dependencies. For a native installation, installing various dependencies of underlying clients, follow the guide below.

## Goal

This guide describes how to setup `xud` and connect to and trade on the XUD Simulation Network (`xud-simnet`). It should take about 15 minutes to complete all steps.

`xud` is in alpha stage and the `xud-simnet` aims to give interested individuals an early 'look & feel' of how things will work lateron with a convenient, mostly automated setup. The `xud-simnet` uses magic simnet coins at all times.

Once the setup of `xud-simnet` is completed, you will be able to query pending orders, place orders, and experiment with `xud`'s rich set of commands (`xucli --help`). When a match is found in the network, your orders will automatically be executed & settled instantly via an atomic swap. `xud-simnet` currently supports atomic swaps between BTC, LTC & ERC20 tokens. This guide will show how to install dependencies like `go` and how to use the `xud-simnet` to automatically setup bitcoin, litecoin & ethereum daemons, lightning daemons (LND) for bitcoin (BTC) and litecoin (LTC), raiden for ERC20 tokens, `xud` and finally connect to `xud-simnet`. This guide is written for Linux, macOS and Windows' [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) and suitable for everyone that knows how to use a command line.

## Describing the Setup

`xud-simnet` spins up several components on your machine which together provide support for exchanging order information and executing atomic swaps between BTC and LTC on the lightning network. It will setup the following components:

- `neutrino` - light node, connecting to our remote btcd instance (which is mining the bitcoin simnet chain)
- `ltcd` - local full node, connected to the litecoin simnet chain
- `geth` - connecting to our remote geth instance (which is mining the ethereum simnet chain)
- `lndbtc` - payment channel implementation for the bitcoin network
- `lndltc` - payment channel implementation for the litecoin network
- `raiden` - payment channel implementation for the ethereum network
- `xud` - Exchange Union's decentralized exchange layer, managing orders and clients above

## Installation 

### Prerequisites

Make sure you have the below programs installed:
- [git](https://gist.github.com/derhuerst/1b15ff4652a867391f03#file-linux-md)
- [make](https://www.cyberciti.biz/faq/debian-linux-install-gnu-gcc-compiler/)
- [node.js + npm](https://github.com/nodesource/distributions/blob/master/README.md#debinstall) (v10.x or higher)
- python 3.7 or higher
- go 1.12 or higher
- killall (`sudo apt-get install psmisc`)

### Installing Go

If Go is not yet installed on your system, use the following command to install it:

```
sudo apt-get install golang-1.12-go
```

If go 1.12 is not included in your package manager, follow the official [install instructions](https://golang.org/doc/install) or add a repository which contains it, e.g. [this one for Ubuntu](https://github.com/golang/go/wiki/Ubuntu). Note, that `golang-1.12-go` puts binaries in `/usr/lib/go-1.10/bin`. If you want them on your PATH, you need to make that change yourself. Alternatively, you can create a softlink with

```
sudo ln -s /usr/lib/go-1.12/bin/go /usr/local/bin/go 
```

### Repository Checkout

```
git clone https://github.com/ExchangeUnion/xud-simnet.git ~/xud-simnet
```

This should create the `xud-simnet` directory in your home directory.

### Setup ~/.bashrc

To enable access to the scripts please update and source your `.bashrc`:

```
source ~/.bashrc
source ~/xud-simnet/setup.bash
```

Please note that the `setup.bash` script sets your `$GOPATH` to `~/xud-simnet/go`. All changes from `setup.bash` are temporary and only active for the current terminal session. You can run

```
echo "source ~/xud-simnet/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

to make these changes permanent.

### Installation

Install all components with

```
xud-simnet-install
```

It could take several minutes, depending on the speed of your machine & connection, until you see `Ready!`.

### Starting

You can start all components with

```
xud-simnet-start
```

Since `neutrino` & `ltcd` need to sync the blocks of the simnet when started for the first time, it could take some minutes until you see `Ready!`. As long as you see the process advancing in the terminal, we'd ask you to be patient and keep the process running.

### Stopping

Gracefully stop the environment with `xud-simnet-stop`.

### Payment Channels

Payment channels are used to instantly settle trades via cross chain atomic swaps with peers. The setup is automated and it can take 10 minutes or more for your payment channels to be ready (the `xud-simnet` features one minute block times).

### Final check

Once you see `Ready!`, run

```
xucli getinfo
```

and you should see something similar to

```
{
  "version": "1.0.0-testnet.1",
  "nodePubKey": "02c8f6f1c2c761b6b61c7cdbf598793a3d0ba73c1ed702498eb51764667567d6c3",
  "urisList": [],
  "numPeers": 3,
  "numPairs": 2,
  "orders": {
    "peer": 40,
    "own": 0
  },
  "lndMap": [
    [
      "BTC",
      {
        "error": "",
        "channels": {
          "active": 1,
          "inactive": 0,
          "pending": 0
        },
        "chainsList": [
          {
            "chain": "bitcoin",
            "network": "simnet"
          }
        ],
        "blockheight": 4916,
        "urisList": [],
        "version": "0.7.1-beta commit=v0.7.1-beta",
        "alias": "BTC@K-Yoga"
      }
    ],
    [
      "LTC",
      {
        "error": "",
        "channels": {
          "active": 1,
          "inactive": 0,
          "pending": 0
        },
        "chainsList": [
          {
            "chain": "litecoin",
            "network": "simnet"
          }
        ],
        "blockheight": 8931,
        "urisList": [],
        "version": "0.7.1-beta commit=v0.7.1-beta",
        "alias": "LTC@K-Yoga"
      }
    ]
  ],
  "raiden": {
    "error": "",
    "address": "0x08D21Ca7E9E306b484A4015984C99eB50e4Cc247",
    "channels": 2,
    "version": ""
  }
}

```

with one active channel for `lndbtc`, `lndltc` & raiden. If so, you are finally ready to rumble. Let's do some trades!

### Trading

`xud` is now connected to `xud-simnet` which includes three permanent nodes operated by Exchange Union. You can view these by running

```
xucli listpeers
```

The output of connected peers should contain

```
"address": "xud1.simnet.exchangeunion.com:8885",
"nodePubKey": "02b66438730d1fcdf4a4ae5d3d73e847a272f160fee2938e132b52cab0a0d9cfc6",

"address": "xud2.simnet.exchangeunion.com:8885",
"nodePubKey": "028599d05b18c0c3f8028915a17d603416f7276c822b6b2d20e71a3502bd0f9e0a"

"address": "xud3.simnet.exchangeunion.com:8885",
"nodePubKey": "03fd337659e99e628d0487e4f87acf93e353db06f754dccc402f2de1b857a319d0"
```

You already received existing orders from the network. You can view these with

```
xucli orderbook
```

Let's execute a test order to trigger a match and execution via atomic swap by e.g. buying 0.5 litecoin with 0.0079 bitcoin

```
xucli buy 0.5 LTC/BTC 0.0079
```

which should result in

```
swapped 0.5 LTC with peer order c0de48a0-0b90-11e9-97c6-95f0014144a4
```

Currently the trading bots of xud1, 2 & 3 are configured to replace successfully orders when the quantity was completely filled. `xud-simnet` features larger BTC and LTC lightning channels to support real-world order sizes.

### Updates & useful commands

`xud-simnet-start` automatically checks for updates and installs these. If an update fails or for any other unexpected errors, please [report the issue](https://github.com/ExchangeUnion/xud-simnet/issues). Optionally use `xud-simnet-stop`, `xud-simnet-clean`, `rm -rf ~/xud-simnet` and install from scratch.

For always up-to-date CLI commands, check `xucli --help`. 

Restart the simnet environment with `xud-simnet-restart`. If you want to control the underlying `lnd` or `ltcd` clients, type `alias` to see aliases set by the `xud-simnet` scripts. to e.g. call `getinfo` for `lndbtc` this would be `lndbtc-lncli getinfo`.

## Help us to improve!

`xud` is in alpha stage, as well is this page. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues), [xud-simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and the [docs](https://github.com/ExchangeUnion/docs/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!