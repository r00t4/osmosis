
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
output
```bash
auth_info:
  fee:
    amount:
    - amount: "5000"
      denom: uosmo
    gas_limit: "2000000"
    granter: ""
    payer: ""
  signer_infos: []
  tip: null
body:
  extension_options: []
  memo: ""
  messages:
  - '@type': /osmosis.tokenfactory.v1beta1.MsgCreateDenom
    sender: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
    subdenom: mykoten
  non_critical_extension_options: []
  timeout_height: "0"
signatures: []
confirm transaction before signing and broadcasting [y/N]: y
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: E81C8C3C0D84F76AAF1294715AA6E98CAEB1EE024715FE0AD2656E4080D171A2
```

3. Verify create denom tx

```bash
./build/osmosisd query tx E81C8C3C0D84F76AAF1294715AA6E98CAEB1EE024715FE0AD2656E4080D171A2
```
output
```bash
code: 0
codespace: ""
data: 12410A342F6F736D6F7369732E746F6B656E666163746F72792E763162657461312E4D736743726561746544656E6F6D526573706F6E736512090A076D796B6F74656E
events:
- attributes:
  - index: true
    key: spender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: amount
    value: 5000uosmo
  type: coin_spent
- attributes:
  - index: true
    key: receiver
    value: osmo17xpfvakm2amg962yls6f84z3kell8c5lczssa0
  - index: true
    key: amount
    value: 5000uosmo
  type: coin_received
- attributes:
  - index: true
    key: recipient
    value: osmo17xpfvakm2amg962yls6f84z3kell8c5lczssa0
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: amount
    value: 5000uosmo
  type: transfer
- attributes:
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  type: message
- attributes:
  - index: true
    key: fee
    value: 5000uosmo
  type: tx
- attributes:
  - index: true
    key: acc_seq
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv/0
  type: tx
- attributes:
  - index: true
    key: signature
    value: 7eO7fYVAydQgTvJ59ZWD2DIDGGHQ0NjvUE7dLkyDp0JlVzOhKjYO6B0DFoZ193+nsk6sPjaPz8IL6KBlDT1tEg==
  type: tx
- attributes:
  - index: true
    key: action
    value: /osmosis.tokenfactory.v1beta1.MsgCreateDenom
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: module
    value: tokenfactory
  - index: true
    key: msg_index
    value: "0"
  type: message
- attributes:
  - index: true
    key: creator
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: new_token_denom
    value: mykoten
  - index: true
    key: msg_index
    value: "0"
  type: create_denom
gas_used: "1092842"
gas_wanted: "2000000"
height: "11"
info: ""
logs: []
raw_log: ""
timestamp: "2024-11-26T13:45:40Z"
tx:
  '@type': /cosmos.tx.v1beta1.Tx
  auth_info:
    fee:
      amount:
      - amount: "5000"
        denom: uosmo
      gas_limit: "2000000"
      granter: ""
      payer: ""
    signer_infos:
    - mode_info:
        single:
          mode: SIGN_MODE_DIRECT
      public_key:
        '@type': /cosmos.crypto.secp256k1.PubKey
        key: A2G5GnZLlHyxQJUI6LW2ww1lnFEBy+3CCl8LsK2OY6Tj
      sequence: "0"
    tip: null
  body:
    extension_options: []
    memo: ""
    messages:
    - '@type': /osmosis.tokenfactory.v1beta1.MsgCreateDenom
      sender: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
      subdenom: mykoten
    non_critical_extension_options: []
    timeout_height: "0"
  signatures:
  - 7eO7fYVAydQgTvJ59ZWD2DIDGGHQ0NjvUE7dLkyDp0JlVzOhKjYO6B0DFoZ193+nsk6sPjaPz8IL6KBlDT1tEg==
txhash: E81C8C3C0D84F76AAF1294715AA6E98CAEB1EE024715FE0AD2656E4080D171A2
```

4. Verify the denom was created in the new format:
```bash
./build/osmosisd query tokenfactory denoms-from-creator osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv --chain-id localosmosis
```
output
```bash
denoms:
- mykoten
```

4. Mint new token
```bash
 ./build/osmosisd tx tokenfactory mint 5000mykoten osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv --from osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv --chain-id localosmosis --fees 5000uosmo --gas auto --keyring-backend test
```
output
```bash
auth_info:
  fee:
    amount:
    - amount: "5000"
      denom: uosmo
    gas_limit: "100000"
    granter: ""
    payer: ""
  signer_infos: []
  tip: null
body:
  extension_options: []
  memo: ""
  messages:
  - '@type': /osmosis.tokenfactory.v1beta1.MsgMint
    amount:
      amount: "5000"
      denom: mykoten
    mintToAddress: osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks
    sender: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  non_critical_extension_options: []
  timeout_height: "0"
signatures: []
confirm transaction before signing and broadcasting [y/N]: y
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: C45A0FA10DB7BA1D7D68427696A6BDE8971DE88C91E1A6AF553603A7079FEFCD
```

5. Verify tx
```bash
./build/osmosisd query tx C45A0FA10DB7BA1D7D68427696A6BDE8971DE88C91E1A6AF553603A7079FEFCD 
```
output
```bash
code: 0
codespace: ""
data: 122F0A2D2F6F736D6F7369732E746F6B656E666163746F72792E763162657461312E4D73674D696E74526573706F6E7365
events:
- attributes:
  - index: true
    key: spender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: amount
    value: 5000uosmo
  type: coin_spent
- attributes:
  - index: true
    key: receiver
    value: osmo17xpfvakm2amg962yls6f84z3kell8c5lczssa0
  - index: true
    key: amount
    value: 5000uosmo
  type: coin_received
- attributes:
  - index: true
    key: recipient
    value: osmo17xpfvakm2amg962yls6f84z3kell8c5lczssa0
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: amount
    value: 5000uosmo
  type: transfer
- attributes:
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  type: message
- attributes:
  - index: true
    key: fee
    value: 5000uosmo
  type: tx
- attributes:
  - index: true
    key: acc_seq
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv/4
  type: tx
- attributes:
  - index: true
    key: signature
    value: i3qBIgiLXgkPJtILho6V179rT0Bdq6lcMyHRL4gZt0Mw7Ku3qwGrxdnC4fhHCRtnIJLZWCn90Saijkq8V7AyTw==
  type: tx
- attributes:
  - index: true
    key: action
    value: /osmosis.tokenfactory.v1beta1.MsgMint
  - index: true
    key: sender
    value: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
  - index: true
    key: module
    value: tokenfactory
  - index: true
    key: msg_index
    value: "0"
  type: message
- attributes:
  - index: true
    key: receiver
    value: osmo19ejy8n9qsectrf4semdp9cpknflld0j64mwamn
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: coin_received
- attributes:
  - index: true
    key: minter
    value: osmo19ejy8n9qsectrf4semdp9cpknflld0j64mwamn
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: coinbase
- attributes:
  - index: true
    key: spender
    value: osmo19ejy8n9qsectrf4semdp9cpknflld0j64mwamn
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: coin_spent
- attributes:
  - index: true
    key: receiver
    value: osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: coin_received
- attributes:
  - index: true
    key: recipient
    value: osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks
  - index: true
    key: sender
    value: osmo19ejy8n9qsectrf4semdp9cpknflld0j64mwamn
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: transfer
- attributes:
  - index: true
    key: sender
    value: osmo19ejy8n9qsectrf4semdp9cpknflld0j64mwamn
  - index: true
    key: msg_index
    value: "0"
  type: message
- attributes:
  - index: true
    key: mint_to_address
    value: osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks
  - index: true
    key: amount
    value: 5000mykoten
  - index: true
    key: msg_index
    value: "0"
  type: tf_mint
gas_used: "91937"
gas_wanted: "100000"
height: "466"
info: ""
logs: []
raw_log: ""
timestamp: "2024-11-26T13:57:15Z"
tx:
  '@type': /cosmos.tx.v1beta1.Tx
  auth_info:
    fee:
      amount:
      - amount: "5000"
        denom: uosmo
      gas_limit: "100000"
      granter: ""
      payer: ""
    signer_infos:
    - mode_info:
        single:
          mode: SIGN_MODE_DIRECT
      public_key:
        '@type': /cosmos.crypto.secp256k1.PubKey
        key: A2G5GnZLlHyxQJUI6LW2ww1lnFEBy+3CCl8LsK2OY6Tj
      sequence: "4"
    tip: null
  body:
    extension_options: []
    memo: ""
    messages:
    - '@type': /osmosis.tokenfactory.v1beta1.MsgMint
      amount:
        amount: "5000"
        denom: mykoten
      mintToAddress: osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks
      sender: osmo18s5lynnmx37hq4wlrw9gdn68sg2uxp5rgk26vv
    non_critical_extension_options: []
    timeout_height: "0"
  signatures:
  - i3qBIgiLXgkPJtILho6V179rT0Bdq6lcMyHRL4gZt0Mw7Ku3qwGrxdnC4fhHCRtnIJLZWCn90Saijkq8V7AyTw==
txhash: C45A0FA10DB7BA1D7D68427696A6BDE8971DE88C91E1A6AF553603A7079FEFCD
```

6. Check balance
```bash
 ./build/osmosisd query bank balances osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks 
```
output
```bash
balances:
- amount: "5000"
  denom: mykoten
- amount: "100000000000"
  denom: stake
- amount: "9999999999999999999999999999999999999999999999999"
  denom: uion
- amount: "9999999999999999999999999999999999999999999999999"
  denom: uosmo
- amount: "100000000000"
  denom: uusdc
- amount: "100000000000"
  denom: uweth
pagination:
  total: "6"
```
---
