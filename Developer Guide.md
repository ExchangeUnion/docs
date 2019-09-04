# Developer Guide

This guide is intended to help developers who want to contribute to `xud`. Developers who want to build on top of `xud`, can check out the [API Documentation](https://api.exchangeunion.com).

## Contribution Guidelines

Be sure to read the [Contribution Guidelines](Contribute.md) for the repository before starting to work or opening a Pull Request.

## Recommended Development Environments

The following development environments are known to be compatible with `xud` and are recommended for developers that are unsure what tools to use.

### Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/) is a cross-platform code editor that's compatible with most popular programming languages and extensible via a large collection of plug-ins. 

#### Visual Studio Code Plugins

Consider using the following plugins for working with `xud`.

- [TSLint](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-tslint-plugin)
- [vscode-proto3](https://marketplace.visualstudio.com/items?itemName=zxh404.vscode-proto3)
- [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=coenraads.bracket-pair-colorizer) (definitely optional)

#### Visual Studio Code Environment Config

Adding the two files from [this gist](https://gist.github.com/sangaman/117af412eefc28c4f763c0152ddd3b99) into a `.vscode` folder within the folder where you've cloned `xud` will automatically provide debug configurations and general settings that are helpful when developing `xud`. 

## `xud` Setup

### Auto-restart `xud` on file change

Auto restart on every file change under `dist` folder with `nodemon`:

```
nodemon --watch dist -e js bin/xud
```

With args:

```
nodemon --watch dist -e js bin/xud --lndbtc.disable=true --lndltc.disable=true
```

### Connect `xud` to the `xud-simnet`

We recommend to connect the `xud` instance you are developing on with the `xud-simnet`. Combined with above restarting mechanism, this let's you instantly see how your changes behave in a real-world trading environment.

1. Install the `xud-simnet` as described [here](https://docs.exchangeunion.com/start-trading/user-guide)
2. Exit from the opened `xud ctl` session by typing `exit`
3. Stop `simnet_xud_1` docker image with `docker stop simnet-xud_1` command, we won't need it.
4. Copy  `~/.xud-docker/simnet/data/lndltc` and `~/.xud-docker/simnet/data/lndbtc` into somewhere you can easily access (such as your home directory). 
5. Ensure you own the copied folders. `chown -R <user>: <copied_path>/lndbtc|lndltc`
6. Change `xud.conf` as below.
```bash
[lnd.BTC]
cltvdelta = 144
disable = false
host = "localhost"
nomacaroons = false
port = 10010
certpath = "<copied_path>/lndbtc/tls.cert"
macaroonpath = "<copied_path>/lndbtc/data/chain/bitcoin/simnet/admin.macaroon"

[lnd.LTC]
cltvdelta = 576
disable = false
host = "localhost"
nomacaroons = false
port = 20010
certpath = "<copied_path>/lndltc/tls.cert"
macaroonpath = "<copied_path>/lndltc/data/chain/litecoin/simnet/admin.macaroon"

[raiden]
disable = false
host = "localhost"
port = 5002
```

7. Now you can start `xud`. Since it's using above configuation, it connects to the running docker environment. Ready to test your changes on xud-simnet!

## References

- [Official TypeScript Documentation](https://www.typescriptlang.org/docs/home.html)
- [Official Node.js Documentation](https://nodejs.org/en/docs/)
- [LND Developer Guide](https://dev.lightning.community/)
- [Official gRPC Documentation](https://grpc.io/docs/)
