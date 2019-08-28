# User Guide

This guide is written for users who want to buy and sell cryptocurrency via `xud`. It explains how to use [`xud-docker`](https://github.com/ExchangeUnion/xud-docker), the recommended and easiest way to get up and running.

## Networks

### Simnet
This is where you want to start. It's quick and gives you a "look and feel". Private chains maintained by Exchange Union cloud instances, we automatically open channels to you and allocate you some precious simnet tokens. Trade against our bots. Consider restarting the setup once after starting for the first time (`down`,  `exit`, `bash ~/xud.sh`, `1`).

Status: `live` | Setup time: `~15 mins` | Recommended available disk space: `5 GB`

### Testnet
bitcoin testnet 3, litecoin testnet 4, ethereum ropsten. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.ropsten.be/) & [2](https://faucet.metamask.io/). Quite a bit of manual work to be done here.

Status: `live` | Setup time: `~5-24h` | Recommended available disk space: `120 GB`

### Mainnet
Real money - only with #reckless hat on.

Status: `live` (with very low limits) | Setup time: `~1-3 days` | Recommended available disk space: `500 GB`

### Regtest
Producing blocks locally, mainly for development

Status: `not available`| Setup time: `fast` | Recommended available disk space: `1 GB`

## Requirements

1. Linux, macOS or [Windows with WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install).

2. 8GB RAM (we saw some weird container states with less on testnet and mainnet). The more, the better. GETH likes RAM. A lot.

3. A SSD. We did not manage to make GETH catch up with the chain when running the container on a regular HDD.

4. docker >= 18.09 & docker-compose >= 1.24. Check with `docker --version` & `docker-compose --version`. If you do not have these installed yet, follow the official install instructions for [docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/).

5. current user can run docker (without sudo). Test with `docker run hello-world`. If this fails, [check these instructions](https://docs.docker.com/install/linux/linux-postinstall/).


## How to run

Start the environment with
```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
 
```
This guides you through a setup on first run, pulls containers, starts syncing chains and opens
```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
--------------------------------------------------------------
```

The `status` command shows status of underlying clients, which is especially useful to track the sync status. If you are syncing full nodes on Testnet/Mainnet, this takes several hours or even days. All clients should show `Ready` before you continue.
```bash
btc	Ready
lndbtc	Ready
ltc	Syncing 2.40% (26887/1116528)
lndltc	Waiting for sync
parity	Syncing 42.61% (2489756/5842588)
raiden	Waiting for sync
xud	Waiting for sync
```

`xud ctl` takes [`xucli` commands](https://api.exchangeunion.com), like `getinfo`. Once everything is up and running, you can check existing orders of your connected peers with `orderbook` and issue an order, e.g. `sell 0.1 btc/dai 9998`. Check your `balance` before and after the swap to see it changing.

## Beware

Raiden currently requires direct channels with trading partners to swap reliably. We have a temporary check in place, that discards raiden-related orders (all pairs which include WETH, DAI...), if `xud` can't find a direct channel to the trading partner. You can switch this check off by setting `raidenDirectChannelChecks=false` in your `xud.conf`. Before you do that, read [this explainer of the issue](https://github.com/ExchangeUnion/xud/issues/1068).

## Other useful things

* Unfortunately docker doesnt play nice with VPN's. We'll fix that in future, for the time being please disconnect VPNs running directly on your system.
* We placed `xud` & `lnd` behind TOR on default, which improves privacy and does away with the need to open ports
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