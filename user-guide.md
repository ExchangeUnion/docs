# User Guide

This page is intended to help users, who want to buy and sell cryptocurrency via `xud`. It explains how to use [`xud-docker`](https://github.com/ExchangeUnion/xud-docker), the recommended and easiest way to get up and running.

## Networks

### Simnet
This is where you want to start to get a "look and feel". Private chains maintained by exchange union cloud instances, automatic opening of channels and allocation of coins & tokens, trading against bots. [Known issues 1](https://github.com/ExchangeUnion/xud-docker/issues/82) & [2](https://github.com/ExchangeUnion/xud-docker/issues/87) currently make it advisable to restart the setup after starting for the first time (`down`, `exit` `bash ~/xud.sh`).

Status: `live` | Setup time: `~15 mins` | Recommended available disk space: `5 GB`

### Testnet
bitcoin testnet 3, litecoin testnet 4, ethereum ropsten. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.ropsten.be/) & [2](https://faucet.metamask.io/). Quite a bit of manual work to be done here.

Status: `live` | Setup time: `~5-10h` | Recommended available disk space: `120 GB`

### Mainnet
Real money - put your #reckless hat on.

Status: `in development` | Setup time: `~1-3 days` | Recommended available disk space: `500 GB`

### Regtest
Producing blocks locally, mainly for development

Status: `not available`| Setup time: `fast` | Recommended available disk space: `1 GB`

## Requirements

1. Linux, macOS or [Windows with WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install).

2. 8GB RAM (we saw some weird container states with less on testnet and mainnet)

3. docker >= 18.09
```
$ docker --version
Docker version 18.09.6, build 481bc77
```
4. docker-compose >= 1.24
```
$ docker-compose --version
docker-compose version 1.24.0, build 0aa59064
```
5. current user can run docker (without sudo)
```
$ docker run hello-world
```
If step 5 fails [check these instructions](https://docs.docker.com/install/linux/linux-postinstall/). If you do not have docker installed yet, follow the official install instructions for [docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/).


## How to run

Start the environment with
```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
 
```
This guides you through a setup on first run, pulls all containers, starts syncing chains and opens
```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
--------------------------------------------------------------
```
`xud ctl` takes [`xucli` commands](https://api.exchangeunion.com), like `getinfo` or `orderbook`. Issue an order with e.g. `sell 0.1 btc/dai 9998`. You can see your `channelbalance` changing after a successful swap.

The `status` command shows status of underlying clients, which is mainly useful to track the sync status on testnet/mainnet:
```bash
btc	Ready
lndbtc	Ready
ltc	Syncing 2.40% (26887/1116528)
lndltc	Waiting for sync
parity	Syncing 42.61% (2489756/5842588)
raiden	Waiting for sync
xud	Waiting for sync
```
## Beware

Raiden currently requires direct channels with trading partners to swap reliably. We have a temporary check in place, that discards raiden-related orders (all pairs which include WETH, DAI...), if `xud` can't find a direct channel to the trading partner. You can switch this check off by setting `raidenDirectChannelChecks=false` in your `xud.conf`. Before you do that, read [this explainer of the issue](https://github.com/ExchangeUnion/xud/issues/1068).

## Other useful things

* We placed `xud` & `lnd` behind TOR on default, which does away with the need to open ports
* `xud ctl` allows to use an underlying client's cli:
```bash
#Simnet
ltcctl --help

#Testnet/Mainnet
bitcoin-cli --help
lndbtc-lncli --help
litecoin-cli --help
lndltc-lncli --help
parity --help
raiden --help
xucli --help
```
* Once everything is up and running, you can check existing orders of your connected peers with `xucli orderbook` and issue e.g. a buy order of 1 ltc with `buy 1 ltc/btc 0.01`.
* Permanently set xud alias to launch `xud ctl` from anywhere:
Add the line `alias xud="bash ~/xud.sh"` to the end of `~/.bashrc`, then run `source ~/.bashrc`.
* To inspect logs (use `logs -f` if you want to follow the log live):
```bash
#Simnet
logs ltcd/geth/lndbtc/lndltc/raiden/xud
#Testnet/Mainnet
logs bitcoind/litecoind/parity/lndbtc/lndltc/raiden/xud
```
* Blockchain & wallet data is stored in
```bash
#Simnet
~/.xud-docker/simnet/data
#Testnet
~/.xud-docker/testnet/data
#Mainnet
~/.xud-docker/mainnet/data
```
* Shutdown environment & remove all data
```bash
docker-compose down
# Use with caution: this step removes all `xud` blockchain and wallet data from your system. If you still have channels open or lost your seed mnemonic, you are risking to loose funds.
rm -rf ~/.xud-docker/
rm -rf ~/xud.sh
```

### References
* [bitcoind config options](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
* [litecoind config options](https://litecoin.info/index.php/Litecoin.conf#litecoin.conf_Configuration_File)
* [parity config options](https://wiki.parity.io/Configuring-Parity-Ethereum)
* [lnd config options](https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf)
* [raiden config options](https://raiden-network.readthedocs.io/en/stable/config_file.html)
* [xud config options](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf)

## Help us to improve!

`xud` is in alpha stage, as well as this wiki. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), [xud-docker](https://github.com/ExchangeUnion/xud-docker/issues), [xud-simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and this [wiki](https://github.com/ExchangeUnion/xud-wiki/issues).

Feel like talking? Chat with us on [Discord](https://discord.gg/YgDhMSn)!