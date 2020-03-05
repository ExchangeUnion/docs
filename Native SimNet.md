We recommend to use the [xud-docker](User%20Guide.md) setup to trade on the `xud-simnet`; it does not require installation of `xud` or any dependencies. For a native installation, installing various dependencies of underlying clients, follow the guide below.

## Goal

This guide describes how to setup `xud` and connect to and trade on the XUD Simulation Network (`xud-simnet`). It should take about 10 minutes to complete all steps.

`xud` is in beta stage and the `xud-simnet` aims to give a 'look & feel' of how things are working with a convenient, automated setup. The `xud-simnet` uses magic simnet coins at all times, no risk of loosing your precious bitcoin if something should go wrong.

Once the setup of `xud-simnet` is completed, you will be able to query the orderbook, place orders, and experiment with `xud`'s rich set of commands (`xucli --help`). When a match is found in the network, your orders will automatically be executed & settled instantly via an atomic swap. `xud-simnet` currently supports atomic swaps between BTC, LTC & ERC20 tokens. This guide will show how to install dependencies like `go` and how to use the `xud-simnet` to set up blightning daemons (LND) for bitcoin (BTC) and litecoin (LTC), raiden for ERC20 tokens and `xud` and to connect to `xud-simnet`. This guide is written for Linux & macOS and suitable for everyone that knows how to use a command line. [Windows WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) support is currently experimental.

## Describing the Setup

`xud-simnet` spins up several components on your machine which together provide support for exchanging order information and executing atomic swaps between BTC,LTC & ERC20 tokens on the lightning network & raiden network. It will setup the following components:

- `lndbtc` - payment channel implementation for the bitcoin network, connecting to our remote btcd instance (which is mining the bitcoin simnet chain) via neutrino
- `lndltc` - payment channel implementation for the litecoin network, connecting to our remote ltcd instance (which is mining the litecoin simnet chain) via neutrino
- `raiden` - payment channel implementation for the ethereum network, connecting to our remote geth instance (which is mining the ethereum simnet chain)
- `xud` - Exchange Union's OpenDEX node, managing orders and clients above

## Installation 

### Prerequisites

Make sure you have the below programs installed:
- [git](https://gist.github.com/derhuerst/1b15ff4652a867391f03#file-linux-md)
- make (`sudo apt-get install make`)
- [node.js + npm](https://github.com/nodesource/distributions/blob/master/README.md#debinstall) (v12.x or higher)
- python 3.7 or higher
- go 1.13 or higher
- killall (`sudo apt-get install psmisc`)

### Installing Go

If Go is not yet installed on your system, use the following command to install it:

```
sudo apt-get install golang-1.13-go
```

If go is not included in your package manager, follow the official [install instructions](https://golang.org/doc/install) or add a repository which contains it, e.g. [this one for Ubuntu](https://github.com/golang/go/wiki/Ubuntu). Note, that `golang-1.13-go` puts binaries in `/usr/lib/go-1.13/bin`. If you want them on your PATH, you need to make that change yourself. Alternatively, you can create a softlink with

```
sudo ln -s /usr/lib/go-1.13/bin/go /usr/local/bin/go 
```

### Repository Checkout

```
git clone https://github.com/ExchangeUnion/xud-simnet.git ~/xud-simnet
```

This clones the `xud-simnet` directory into your home directory.

### Setup ~/.bashrc

To enable access to the scripts from anywhere run:

```
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

Since `neutrino` needs to sync the blocks of the simnet when started for the first time, it could take some minutes until you see `Ready!`. As long as you see the process advancing in the terminal, we'd ask you to be patient and keep the process running.

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
General XUD Info
┌───────────────┬────────────────────────────────────────────────────────────────────┐
│ Status        │ Ready                                                              │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Alias         │                                                                    │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Node Key      │ 03fe7f3b78cb5cba37d8c60318f612361bcb1a85d09498a5b46ea0f4ac399902ed │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Address       │                                                                    │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Network       │ simnet                                                             │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Version       │ 1.0.0-beta.1-da08b2a                                               │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Peers         │ 1                                                                  │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Pairs         │ 4                                                                  │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Own orders    │ 0                                                                  │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Peer orders   │ 3                                                                  │
├───────────────┼────────────────────────────────────────────────────────────────────┤
│ Pending swaps │ []                                                                 │
└───────────────┴────────────────────────────────────────────────────────────────────┘ 


Raiden info:
┌──────────┬────────────────────────────────────────────┐
│ Status   │ raiden has no active channels              │
├──────────┼────────────────────────────────────────────┤
│ Version  │ 0.100.5a1.dev162-2217bcb                   │
├──────────┼────────────────────────────────────────────┤
│ Address  │ 0xE63eff732aA623300EAe509D1ADF9436295B543c │
├──────────┼────────────────────────────────────────────┤
│ Channels │ Active: 2 | Pending: 0 | Closed: 0         │
├──────────┼────────────────────────────────────────────┤
│ Network  │ raiden simnet                              │
└──────────┴────────────────────────────────────────────┘ 


LND-BTC Info:
┌──────────┬────────────────────────────────────┐
│ Status   │ Ready                              │
├──────────┼────────────────────────────────────┤
│ Version  │ 0.9.1-beta commit=v0.9.1-beta      │
├──────────┼────────────────────────────────────┤
│ Address  │                                    │
├──────────┼────────────────────────────────────┤
│ Alias    │ BTC@K-Yoga                         │
├──────────┼────────────────────────────────────┤
│ Channels │ Active: 1 | Pending: 0 | Closed: 0 │
├──────────┼────────────────────────────────────┤
│ Network  │ bitcoin simnet                     │
└──────────┴────────────────────────────────────┘ 


LND-LTC Info:
┌──────────┬────────────────────────────────────┐
│ Status   │ Ready                              │
├──────────┼────────────────────────────────────┤
│ Version  │ 0.9.0-beta commit=v0.9.0-beta      │
├──────────┼────────────────────────────────────┤
│ Address  │                                    │
├──────────┼────────────────────────────────────┤
│ Alias    │ LTC@K-Yoga                         │
├──────────┼────────────────────────────────────┤
│ Channels │ Active: 1 | Pending: 0 | Closed: 0 │
├──────────┼────────────────────────────────────┤
│ Network  │ litecoin simnet                    │
└──────────┴────────────────────────────────────┘ 
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
```

You already received existing orders from the network. You can view these with

```
xucli orderbook
```

Let's execute a test order to trigger a match and execution via atomic swap by e.g. buying 0.5 litecoin for a price of 0.0079 bitcoin per litecoin

```
xucli buy 0.5 LTC/BTC 0.0079
```

which should result in

```
swapped 0.5 LTC with peer order c0de48a0-0b90-11e9-97c6-95f0014144a4
```

Currently the trading bots of xud1 is configured to replace successfully orders when the quantity was completely filled. `xud-simnet` features larger BTC and LTC lightning channels to support real-world order sizes.

### Updates & useful commands

`xud-simnet-start` automatically checks for updates and installs these. If an update fails or for any other unexpected errors, please [report the issue](https://github.com/ExchangeUnion/xud-simnet/issues). Optionally use `xud-simnet-stop`, `xud-simnet-clean`, `rm -rf ~/xud-simnet` and install from scratch.

For always up-to-date CLI commands, check `xucli --help`. 

Restart the simnet environment with `xud-simnet-restart`. If you want to control the underlying `lnd` or `ltcd` clients, type `alias` to see aliases set by the `xud-simnet` scripts. to e.g. call `getinfo` for `lndbtc` this would be `lndbtc-lncli getinfo`.
