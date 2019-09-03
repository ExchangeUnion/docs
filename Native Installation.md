# Native Installation for Development

This page contains instructions for installing `xud` natively, without treating setup of dependencies like `bitcoind` or `lnd`. It is mainly geared towards developers. For all other users, we recommend our streamlined setup via [xud-docker](User%20Guide.md).

## Requirements

Make sure to have the folloiwng installed:
- [Node.js](https://nodejs.org/en/download/), latest LTS or newer
- [Python](https://www.python.org/), v2.7.16
- [Go](https://golang.org/), v1.12 or higher

## Cloning from GitHub

Testers and developers should clone the repository from GitHub and install dependencies.

```bash
git clone https://github.com/ExchangeUnion/xud
cd xud
npm install
npm run compile
npm run compile:seedutil
```

# Installing from npm
```bash
sudo npm install xud -g --unsafe-perm
```

## Daemonize `xud`

If you want to daemonize `xud` so it starts on boot without needing its own terminal, you can do this using `systemd`. The following sample `systemd` configuration was tested on Ubuntu 18.04:

```bash
[Unit]
Description= XUD - ExchangeUnion Daemon
ConditionPathExists=/opt/xud/dist/
After=network.target

[Service]
User=xud
Group=xud
WorkingDirectory=/opt/xud/
ExecStart=/usr/bin/node /opt/xud/dist/Xud.js
Type=simple

[Install]
WantedBy=multi-user.target
```