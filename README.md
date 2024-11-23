
# Osmosis

![Banner](assets/banner.png)

[![Project Status: Active](https://img.shields.io/badge/repo%20status-Active-green.svg?style=flat-square)](https://www.repostatus.org/#active)
[![GoDoc](https://img.shields.io/badge/godoc-reference-blue?style=flat-square&logo=go)](https://pkg.go.dev/github.com/osmosis-labs/osmosis/v11)
[![Go Report Card](https://goreportcard.com/badge/github.com/osmosis-labs/osmosis?style=flat-square)](https://goreportcard.com/report/github.com/osmosis-labs/osmosis/v11)
[![Version](https://img.shields.io/github/tag/osmosis-labs/osmosis.svg?style=flat-square)](https://github.com/osmosis-labs/osmosis/releases/latest)
[![License: Apache-2.0](https://img.shields.io/github/license/osmosis-labs/osmosis.svg?style=flat-square)](https://github.com/osmosis-labs/osmosis/blob/main/LICENSE)
[![Lines Of Code](https://img.shields.io/tokei/lines/github/osmosis-labs/osmosis?style=flat-square)](https://github.com/osmosis-labs/osmosis)
[![Lint](https://img.shields.io/github/actions/workflow/status/osmosis-labs/osmosis/lint.yml?style=flat-square&label=Lint)](https://github.com/marketplace/actions/super-linter)
[![Discord](https://badgen.net/badge/icon/discord?icon=discord&label)](https://discord.gg/osmosis)

---

## Changes in This Fork

This fork introduces a modification to the **denom creation function**.

### Key Change: Modified Denom Creation Function

The denom string for tokens created by the `tokenfactory` now only includes the subdenom.

**Old Format**: `factory/{creator}/{subdenom}`  
**New Format**: `{subdenom}`

---

## Prerequisites

Ensure you have Docker and Docker Compose installed:

```bash
# Install Docker
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt install docker.io -y

# Install Docker Compose
sudo apt install docker-compose -y
```

---

## Before Testing Changes

To test the changes, you need to configure and start the test environment. Follow these steps:

0. Build new binary file
```bash
make build 
```
After this, you will have new binary in ./build folder

Optionally, remove old configs
```bash
rm -rf ~/.osmosisd
rm -rf ~/.osmosisd-local 
```

1. Initialize the environment:
```bash
make localnet-init
```

2. Start the test network, in detached mode:
```bash
make localnet-startd
```

3. Add validator and test wallets:
```bash
make localnet-keys
```

---

## Testing changes

1. Create a new denom:
```bash
./build/osmosisd tx tokenfactory create-denom mykoten --from lo-test2 --chain-id localosmosis --fees 5000uosmo --gas 2000000 --keyring-backend test
```

2. Verify the denom was created in the new format:
```bash
./build/osmosisd query tokenfactory denoms-from-creator osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv --chain-id localosmosis
```

---
