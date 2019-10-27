# Reference

*Read this in other languages: English, 简体中文(TBD).*
## Command Line Interface (CLI)

CLI tool (bhcli) provide the interaction with Bluehelix Chain, including queries and submitting transactions.

The tool is executed as

```console
$ docker-compose run bhcli ...
```

## Getting help

For highest level help, just run

```console
$ docker-compose run bhcli help
bhclear client

Usage:
  bhcli [command]

Available Commands:
  status      Query remote node for status
  query       Querying subcommands
  tx          Transactions subcommands

  keys        Add or view local private keys
  cu          Add or view custodian unit info from local private keys

  help        Help about any command

Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
  -h, --help              help for bhcli
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli [command] --help" for more information about a command.
```

For information about specific command, use
```console
$ docker-compose run bhcli [command] --help
```

## Local keystore and custodian unit information

TBD

## Query chain states

TBD

## Generate and broadcast transactions

subcommand 'tx' can Generate and broadcast transactions.
```console
$ docker-compose run bhcli tx -h
Transactions subcommands

Usage:
  bhcli tx [command]

Available Commands:
  sign        Sign transactions generated offline
  multisign   Generate multisig signatures for transactions
  broadcast   Broadcast transactions generated offline

  cu          cu subcommands
  token       token transaction subcommands
  asset       tx subcommands
  send        Create and sign a send tx
  distr       Distribution transactions subcommands

Flags:
  -h, --help   help for tx

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Additional help topics:
  bhcli tx

Use "bhcli tx [command] --help" for more information about a command.
```
### Sign transactions generated offline
```console
$ docker-compose run bhcli tx sign -h
Sign transactions created with the --generate-only flag.
It will read a transaction from [file], sign it, and print its JSON encoding.

If the flag --signature-only flag is set, it will output a JSON representation
of the generated signature only.

If the flag --validate-signatures is set, then the command would check whether all required
signers have signed the transactions, whether the signatures were collected in the right
order, and if the signature is valid over the given transaction. If the --offline
flag is also set, signature validation over the transaction will be not be
performed as that will require RPC communication with a full node.

The --offline flag makes sure that the client will not reach out to full node.
As a result, the account and sequence number queries will not be performed and
it is required to set such parameters manually. Note, invalid values will cause
the transaction to fail.

Usage:
  bhcli tx sign [file] [flags]

Flags:
  -a, --account-number uint      The account number of the signing account (offline mode only)
      --append                   Append the signature to the existing ones. If disabled, old signatures would be overwritten. (default true)
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                  ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string              Fees to pay along with transaction; eg: 10uatom
      --from string              Name or address of private key with which to sign
      --gas string               gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only            Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                     help for sign
      --indent                   Add indent to JSON response
      --ledger                   Use a connected Ledger device
      --memo string              Memo to send along with transaction
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --offline                  Offline mode; Do not query a full node
      --output-document string   The document will be written to the given file instead of STDOUT
      --print-response           return tx response (only works with async = false) (default true)
  -s, --sequence uint            The sequence number of the signing account (offline mode only)
      --signature-only           Print only the generated signature, then exit
      --trust-node               Trust connected full node (don't verify proofs for responses) (default true)
      --validate-signatures      Print the addresses that must sign the transaction, those who have already signed it, and make sure that signatures are in the correct order
  -y, --yes                      Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Generate multisig signatures for transactions
```console
$ docker-compose run bhcli tx multisign -h
Sign transactions created with the --generate-only flag that require multisig signatures.

Read signature(s) from [signature] file(s), generate a multisig signature compliant to the
multisig key [name], and attach it to the transaction read from [file]. Example:

   bhcli multisign transaction.json k1sig.json k2sig.json k3sig.json

If the flag --signature-only flag is on, it outputs a JSON representation
of the generated signature only.

Usage:
  bhcli tx multisign [file] [[signature]...] [flags]

Flags:
  -a, --account-number uint      The account number of the signing account (offline mode only)
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                  ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string              Fees to pay along with transaction; eg: 10uatom
      --from string              Name or address of private key with which to sign
      --gas string               gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only            Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                     help for multisign
      --indent                   Add indent to JSON response
      --ledger                   Use a connected Ledger device
      --memo string              Memo to send along with transaction
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --output-document string   The document will be written to the given file instead of STDOUT
      --print-response           return tx response (only works with async = false) (default true)
  -s, --sequence uint            The sequence number of the signing account (offline mode only)
      --signature-only           Print only the generated signature, then exit
      --trust-node               Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                      Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Broadcast transactions generated offline
```concole
$ docker-compose run bhcli tx broadcast -h
Broadcast transactions created with the --generate-only
flag and signed with the sign command. Read a transaction from [file_path] and
broadcast it to a node. If you supply a dash (-) argument in place of an input
filename, the command reads from standard input.

$ gaiacli tx broadcast ./mytxn.json

Usage:
  bhcli tx broadcast [file_path] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for broadcast
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### custodian unit 

```console
$ docker-compose run bhcli tx cu -h
cu subcommands

Usage:
  bhcli tx cu [command]

Available Commands:
  keygen       keygen
  keygenfinish keygenfinish
  keygencancel keygencancel

Flags:
  -h, --help   help for cu

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli tx cu [command] --help" for more information about a command.
```

#### custodian unit keygen
```console
$ docker-compose run bhcli tx cu keygen -h
 Example: keygen BHBTC BHPSfYjrgEgM97gpCWRd2UStvRVRqFgw6mQ

Usage:
  bhcli tx cu keygen [symbol] [to] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for keygen
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### custodian unit keygenfinish
```concole
$ docker-compose run bhcli tx cu keygenfinish -h
Example: 
  keygenfinish 1 BHPSfYjrgEgM97gpCWRd2UStvRVRqFgw6mQ  mhoGjKn5xegDXL6u5LFSUQdm5ozdM6xao9 BHPSfYjrgEgM97gpCWRd2UStvRVRqFgw6mQ
        BHVw8uKtTFJRR7Xw2L98pQkmtEBM4btWhTB

Usage:
  bhcli tx cu keygenfinish [order-id] [order_from] [address] []key_nodes [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for keygenfinish
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### custodian unit keygencancel
```concole
$ docker-compose run bhcli tx cu keygencancel -h

Example:
  key_gen_cancel order_id

Usage:
  bhcli tx cu keygencancel [order-id] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for keygencancel
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### token transaction subcommands
```console
$ docker-compose run bhcli tx token -h
token transaction subcommands

Usage:
  bhcli tx token [command]

Available Commands:
  token-keygen       token-keygen
  token-keygenfinish token-keygenfinish
  token-keygencancel token-keygencancel

Flags:
  -h, --help   help for token

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli tx token [command] --help" for more information about a command.
```

#### token token-keygen
```console
$ docker-compose run bhcli tx token token-keygen -h

		Example: token-keygen symbol address_type

Usage:
  bhcli tx token token-keygen [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for token-keygen
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

#### token token-keygenfinish
```console
$ docker-compose run bhcli tx token token-keygenfinish -h

		Example: token-keygenfinish order_id to address []key_nodes

Usage:
  bhcli tx token token-keygenfinish [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for token-keygenfinish
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### token token-keygencancel
```console
$ docker-compose run bhcli tx token token-keygencancel -h

		Example: token-keygencancel order_id

Usage:
  bhcli tx token token-keygencancel [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for token-keygencancel
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### asset
```console
bhcli tx asset  -h
tx subcommands

Usage:
  bhcli tx asset [command]

Available Commands:
  deposit           Deposit asset to BHEX Chain
  collect-start     collect-start to sign tx for collect asset from sepecified address to withdrawal address
  collect-finish    collect-finish collect asset, the unspent transaction fee will be return
  collect-cancel    collect-cancel collect asset, the collect order will be canceled
  withdrawal-start  withdrawal-start to sign tx for withdrawal asset from sepecified address to withdrawal address
  withdrawal-finish withdrawal-finish withdrawal asset, the unspent transaction fee will be return
  withdrawal-cancel withdrawal-cancel withdrawal asset, the withdrawal order will be canceled

Flags:
  -h, --help   help for asset

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli tx asset [command] --help" for more information about a command.
```

#### asset deposit
```concole
$ docker-compose run bhcli tx asset deposit -h
  Deposit asset to BHEX Chain, and BHEX Chain will check it through the real chain.
  Example: bhcli tx asset deposit --from alice  --chain-id bhexchain 100bheth 5377064 0x2e9a512fc6fea120e567ed5faef1440e4f66b5ff 0x1b5894be4f66eb75a63b5010db4a0a5cfe0b589956b74086bf64585939da1659

Usage:
  bhcli tx asset deposit [coin] [blockheight] [address] [txhash] [utxoins] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for deposit
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset collect-start
```console
$ docker-compose run bhcli tx asset collect-start -h
  collect-start to sign tx for collect asset from sepecified address to withdrawal address
  Example: bhcli tx asset collect-start --from alice --chain-id bhexchain 100bheth 633578 21000 10000 BHKy7eQidQcEKG1nA24bdPDR2nNRokR9TEg 0x15a09efb2d85c888fa8960941f1fffe5251bb37b 0x3e7e53729c21b4ccba3e8637cca5d3875475731f

Usage:
  bhcli tx asset collect-start [coin] [blockHeight] [gasUnit] [gasPrice] [collectFrom] [fromAddress] [toAddress] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for collect-start
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset collect-finish
```console
$ docker-compose run bhcli tx asset collect-finish -h
  collect-finish collect asset, the unspent transaction fee will be return.
  Example: bhcli tx asset collect-finish  --from alice  --chain-id bhexchain 3526 21000 5377064 0x1b5894be4f66eb75a63b5010db4a0a5cfe0b589956b74086bf64585939da1659

Usage:
  bhcli tx asset collect-finish [OrderID] [GasUsed] [BlockNumber] [TxHash] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for collect-finish
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset collect-cancel
```console
$ docker-compose run bhcli tx asset collect-cancel -h
  collect-cancel collect asset, the collect order will be canceled.
  Example: bhcli tx asset collect-cancel  --from alice  --chain-id bhexchain 2235

Usage:
  bhcli tx asset collect-cancel [orderID] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for collect-cancel
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset withdrawal-start
```console
bhcli tx asset withdrawal-start -h
  withdrawal-start to sign tx for withdrawal asset from sepecified address to withdrawal address
  Example: bhcli tx asset withdrawal-start --from alice --chain-id bhexchain 100bheth  633578 1 21000 0x15a09efb2d85c888fa8960941f1fffe5251bb37b 0x3e7e53729c21b4ccba3e8637cca5d3875475731f

Usage:
  bhcli tx asset withdrawal-start [coin] [blockHeight] [gasUnit] [gasPrice] [fromAddress] [toAddress] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for withdrawal-start
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset withdrawal-finish
```console
$ docker-compose run bhcli tx asset withdrawal-finish -h
  withdrawal-finish withdrawal asset, the unspent transaction fee will be return.
  Example: bhcli tx asset withdrawal-finish  --from alice  --chain-id bhexchain 3526 21000 5377064 0x1b5894be4f66eb75a63b5010db4a0a5cfe0b589956b74086bf64585939da1659

Usage:
  bhcli tx asset withdrawal-finish [OrderID] [GasUsed] [BlockNumber] [TxHash] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for withdrawal-finish
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### asset withdrawal-cancel
```console
$ docker-compose run bhcli tx asset withdrawal-cancel -h
  withdrawal-cancel withdrawal asset, the withdrawal order will be canceled.
  Example: bhcli tx asset withdrawal-cancel  --from alice  --chain-id bhexchain 2356

Usage:
  bhcli tx asset withdrawal-cancel [orderID] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for withdrawal-cancel
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Create and sign a send tx
```console
$ docker-compose run bhcli tx send  -h
Create and sign a send tx

Usage:
  bhcli tx send [to_address] [coin] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for send
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Distribution transactions
```console
$ docker-compose run bhcli tx distr -h
Distribution transactions subcommands

Usage:
  bhcli tx distr [command]

Available Commands:
  withdraw-rewards     withdraw rewards from validator-addr to delegator designated address(set by set-withdraw-addr), and optionally withdraw validator's commission if delegator is a validator operator
  set-withdraw-addr    set withdraw-addr on behalf of delegator
  withdraw-all-rewards withdraw all delegations rewards for a delegator

Flags:
  -h, --help   help for distr

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli tx distr [command] --help" for more information about a command.
```

#### Distribution withdraw-rewards
```console
$ docker-compose run bhcli tx distr withdraw-rewards -h
witdraw rewards from validator-addr to delegator address, and optionally withdraw validator's commission also if delegator is a validator operator:
		$ bhcli tx distr withdraw-rewards BHfeAYhzf54jNp7vWmvVapgjgsoEr8P2spF --from jack
		$ bhcli tx distr withdraw-rewards BHfeAYhzf54jNp7vWmvVapgjgsoEr8P2spF --from alice --commission  //if alice is a  val operator

Usage:
  bhcli tx distr withdraw-rewards [validator-addr] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --commission              also withdraw validator's commission
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for withdraw-rewards
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Distribution set-withdraw-addr
```console
$ docker-compose run bhcli tx distr set-withdraw-addr -h
set withdraw-addr on behalf of delegator, default  withdrawal addr = delegator address:
		$ bhcli tx set-withdraw-addr BHZHjUFBF3sgx2kpWEjsMGgGMSJRzohb8aa --from alice

Usage:
  bhcli tx distr set-withdraw-addr [withdraw-addr] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for set-withdraw-addr
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Distribution withdraw-all-rewards
```console
$ docker-compose run bhcli tx distr withdraw-all-rewards -h
Withdraw all rewards for a single delegator:
		$ bhcli tx distr withdraw-all-rewards --from alice

Usage:
  bhcli tx distr withdraw-all-rewards [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 10uatom
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 10uatom)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                    help for withdraw-all-rewards
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --max-msgs int            Limit the number of messages per tx (0 for unlimited) (default 5)
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --print-response          return tx response (only works with async = false) (default true)
  -s, --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/admin/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

