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

### Keystore management

The subcommand `keys` allows you to manage your local keystore. To get help, just run:

```console
$ docker-compose run bhcli keys -h
Keys allows you to manage your local keystore for tendermint.

    These keys may be in any format supported by go-crypto and can be
    used by light-clients, full nodes, or any other application that
    needs to sign with a private key.

Usage:
  bhcli keys [command]

Available Commands:
  mnemonic    Compute the bip39 mnemonic for some input entropy
  add         Add an encrypted private key (either newly generated or recovered), encrypt it, and save to disk
  list        List all keys
  show        Show key info for the given name
              
  delete      Delete the given key
  update      Change the password used to protect private key

Flags:
  -h, --help   help for keys

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli keys [command] --help" for more information about a command.

```
#### Create a bip39 mnemonic

Mnemonic can easily restore your private key.

```console
 $ docker-compose run bhcli keys mnemonic -h
Create a bip39 mnemonic, sometimes called a seed phrase, by reading from the system entropy. To pass your own entropy, use --unsafe-entropy

Usage:
  bhcli keys mnemonic [flags]

Flags:
  -h, --help             help for mnemonic
      --unsafe-entropy   Prompt the user to supply their own entropy, instead of relying on the system

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

#### Create a new private key

```console
$ docker-compose run bhcli keys add -h
Derive a new private key and encrypt to disk.
Optionally specify a BIP39 mnemonic, a BIP39 passphrase to further secure the mnemonic,
and a bip32 HD path to derive a specific account. The key will be stored under the given name
and encrypted with the given password. The only input that is required is the encryption password.

If run with -i, it will prompt the user for BIP44 path, BIP39 mnemonic, and passphrase.
The flag --recover allows one to recover a key from a seed passphrase.
If run with --dry-run, a key would be generated (or recovered) but not stored to the
local keystore.
Use the --pubkey flag to add arbitrary public keys to the keystore for constructing
multisig transactions.

You can add a multisig key by passing the list of key names you want the public
key to be composed of to the --multisig flag and the minimum number of signatures
required through --multisig-threshold. The keys are sorted by address, unless
the flag --nosort is set.

Usage:
  bhcli keys add <name> [flags]

Flags:
      --account uint32            Account number for HD derivation
      --dry-run                   Perform action, but don't add key to local keystore
  -h, --help                      help for add
      --indent                    Add indent to JSON response
      --index uint32              Address index number for HD derivation
  -i, --interactive               Interactively prompt user for BIP39 passphrase and mnemonic
      --ledger                    Store a local reference to a private key on a Ledger device
      --multisig strings          Construct and store a multisig public key (implies --pubkey)
      --multisig-threshold uint   K out of N required signatures. For use in conjunction with --multisig (default 1)
      --no-backup                 Don't print out seed phrase (if others are watching the terminal)
      --nosort                    Keys passed to --multisig are taken in the order they're supplied
      --pubkey string             Parse a public key in bech32 format and save it to disk
      --recover                   Provide seed phrase to recover existing key instead of creating

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

Example

```console
$ docker-compose run bhcli keys add alice
Enter a passphrase to encrypt your key to disk:
Repeat the passphrase:
NAME:	TYPE:	ADDRESS:				PUBKEY:
alice	local	cosmos1j52w984y2454lsllk3g9s90hud9gn8crlur5cp	cosmospub1addwnpepqvu7jx79skgyr9ma4zgtdwhd8sklau646sqsruekf7l2fug0eqs0zaagmrp
```

Please ignore the output address and pubkey as we are using cosmos-sdk to manage keystore. Insterad, we can run command `cu show` to get Bluehelix Chain custodian unit address.

#### List all keys 

Subcommand `list` can list all public keys. Note that this command only list cosmos addresses and pubkeys, to get custodian unit addresses, please refer to `cu list` command.

```console
$ docker-compose run bhcli keys list -h
Return a list of all public keys stored by this key manager
along with their associated name and address.

Usage:
  bhcli keys list [flags]

Flags:
  -h, --help     help for list
      --indent   Add indent to JSON response

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

#### Show keyname info

Subcommand `show` can show the designated keyname info, similiar the `list` subcommand. Note that this command only shows cosmos address and pubkey, to get custodian unit address, please refer to `cu show` command.

```console
$ docker-compose run bhcli keys show bolen
NAME:   TYPE:   ADDRESS:                                        PUBKEY:
bolen   local   cosmos1rknv9e5akqynqexexkp5juvrd3elv69hrdr2zr   cosmospub1addwnpepq0xecnv35txrecwm0a22x6l82v92a4shjuy0ut560mwxhhwaqr205y8l7ys

$ docker-compose run bhcli keys list bolen
NAME:   TYPE:   ADDRESS:                                        PUBKEY:
bolen   local   cosmos1rknv9e5akqynqexexkp5juvrd3elv69hrdr2zr   cosmospub1addwnpepq0xecnv35txrecwm0a22x6l82v92a4shjuy0ut560mwxhhwaqr205y8l7ys

```

#### Delete the given key

```console
$ docker-compose run bhcli keys delete -h
Delete a key from the store.

Note that removing offline or ledger keys will remove
only the public key references stored locally, i.e.
private keys stored in a ledger device cannot be deleted with
gaiacli.

Usage:
  bhcli keys delete <name> [flags]

Flags:
  -f, --force   Remove the key unconditionally without asking for the passphrase
  -h, --help    help for delete
  -y, --yes     Skip confirmation prompt when deleting offline or ledger key references

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

#### Update the password of protecting private key

```console
$ docker-compose run bhcli keys update -h
Change the password used to protect private key

Usage:
  bhcli keys update <name> [flags]

Flags:
  -h, --help   help for update

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```
**For instance:**

```console
$ docker-compose run bhcli keys update bolen -o text
Enter the current passphrase:
Enter the new passphrase:
Repeat the new passphrase:
Password successfully updated!

```

### Local keystore custodian unit information

The subcommand `cu` allows you to get information of custodian units from your local keystore. To get help, just run:

```console
$ docker-compose run bhcli cu help
Add or view custodian unit info from local private keys

Usage:
  bhcli cu [command]

Available Commands:
  show        Show custodian unit addresses or public keys for one or more key names
  list        list custodian unit addresses for all key names

Flags:
  -h, --help   help for cu

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli cu [command] --help" for more information about a command.
```
#### List all local custodian units

The subcommand `cu list` will list all cu accounts
```console
$ docker-compose run bhcli cu list -h
list custodian unit addresses for all key names

Usage:
  bhcli cu list [flags]

Flags:
  -h, --help   help for list

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```
For instance:

```console
$ docker-compose run bhcli cu list
Custodian Unit Address for bolen: BHNbhbGfZwcZeakMHPUaYARdZquUdiBahYq
Custodian Unit Public Key for bolen: BHPubKey:bpJuCxZghLVMU4xsYEPwiKDFWC3HoL9scsk5uK2RBQL4MqbcCzND

```

#### Show information of a local custodian unit

The subcommand `cu show` will show designate custodian unit info

```console
$ docker-compose run bhcli cu show -h
Show custodian unit addresses or public keys for one or more key names

Usage:
  bhcli cu show [name [name...]] [flags]

Flags:
  -a, --cu_address   Output the address only (overrides --output)
  -h, --help         help for show
      --indent       Add indent to JSON response
  -p, --pubkey       Output the public key only (overrides --output)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```

## Generate and broadcast transactions

Subcommand 'tx' can generate and broadcast transactions.
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

### Transactions for custodian units

To get help, run the following command

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

#### Generate a deposit address for a custodian unit

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

### Transactions for assets

To get help, run the following command

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

#### Deposit an asset

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

#### Start collecting an asset to a toekn withdrawal address

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

#### Finish collecting an asset

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

#### Start withdrawal of an asset from a token withdrawal address

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

#### Finish withdrawal of an asset

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

### Send an asset to another custodian unit

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

## Query chain states

To get help, run the following commands

```
$docker-compose run bhcli query -h
Querying subcommands

Usage:
  bhcli query [command]

Aliases:
  query, q

Available Commands:
  tendermint-validator-set Get the full tendermint validator set at given height
  block                    Get verified data for a the block at given height
  txs                      Search for paginated transactions that match a set of tags
  tx                       Find a transaction by hash in a committed block.

  cu                       Querying commands for the custodianunit module
  token                    Querying commands for the token module
  address                  Querying commands for the address module
  distr                    Querying commands for the distribution module

Flags:
  -h, --help   help for query

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Additional help topics:
  bhcli query
  bhcli query

Use "bhcli query [command] --help" for more information about a command.
```

### Retrieve tendermint validator set at given height
```
$docker-compose run bhcli query tendermint-validator-set -h
Get the full tendermint validator set at given height

Usage:
  bhcli query tendermint-validator-set [height] [flags]

Flags:
  -h, --help          help for tendermint-validator-set
      --indent        indent JSON response
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Retrieve block contents at given height
```
$docker-compose run bhcli query block -h
Get verified data for a the block at given height

Usage:
  bhcli query block [height] [flags]

Flags:
  -h, --help          help for block
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
### Search for transaction that match the specified tags 
```
$docker-compose run bhcli query txs -h
Search for transactions that match the exact given tags where results are paginated.

Example:
$ gaiacli query txs --tags '<tag1>:<value1>&<tag2>:<value2>' --page 1 --limit 30

Usage:
  bhcli query txs [flags]

Flags:
  -h, --help           help for txs
      --limit uint32   Query number of transactions results per page returned (default 30)
  -n, --node string    Node to connect to (default "tcp://localhost:26657")
      --page uint32    Query a specific page of paginated results (default 1)
      --tags string    tag:value list of tags that must match
      --trust-node     Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Search a transaction by hash
```
$docker-compose run bhcli query tx -h
Find a transaction by hash in a committed block.

Usage:
  bhcli query tx [hash] [flags]

Flags:
  -h, --help          help for tx
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Custodian unit query commands

```
$docker-compose run bhcli query cu -h
Querying commands for the custodianunit module

Usage:
  bhcli query cu [command]

Available Commands:
  info        Query custodian unit info
  assets      Query assets info of a custodian unit
  asset       Query asset info of a custodian unit
  order       Query custodianuint order by ID
  orders      Query custodianuint orders

Flags:
  -h, --help   help for cu

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli query cu [command] --help" for more information about a command.
```

#### Query a custodian unit infomation
```
$docker-compose run bhcli query cu info -h
Query custodian unit info

Usage:
  bhcli query cu info [cuaddress] [flags]

Flags:
  -h, --help          help for info
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Query all assets of a custodian unit 
```
$docker-compose run bhcli query cu assets -h
Query assets info of a custodian unit

Usage:
  bhcli query cu assets [cuaddress] [flags]

Flags:
  -h, --help          help for assets
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Query an asset of a custodian unit by symbol
```
$docker-compose run bhcli query cu asset -h
Query asset info of a custodian unit

Usage:
  bhcli query cu asset [cuaddress] [symbol] [flags]

Flags:
  -h, --help          help for asset
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Query all orders of a custodian unit
```
$docker-compose run bhcli query cu orders -h
Query custodianuint orders

Usage:
  bhcli query cu orders [cuaddress]  [flags]

Flags:
  -h, --help          help for orders
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Query an order of a custodian unit by order id
```
$docker-compose run bhcli query cu order -h
Query custodianuint order by ID

Usage:
  bhcli query cu order [cuaddress] [id] [flags]

Flags:
  -h, --help          help for order
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

### Token query commands
```
$docker-compose run bhcli query token -h
Querying commands for the token module

Usage:
  bhcli query token [command]

Available Commands:
  token       token symbol
  tokens      tokens
  decimal     decimal symbol

Flags:
  -h, --help   help for token

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

Use "bhcli query token [command] --help" for more information about a command.
```

#### Retrieve a token information by symbol
```
$docker-compose run bhcli query token token -h
token symbol

Usage:
  bhcli query token token [symbol] [flags]

Flags:
  -h, --help          help for token
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Retrieve all supported tokens name 
```
$docker-compose run bhcli query token tokens -h
tokens

Usage:
  bhcli query token tokens [flags]

Flags:
  -h, --help          help for tokens
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### Retrieve a token's decimal by symbol
```
$docker-compose run bhcli query token decimal -h
decimal symbol

Usage:
  bhcli query token decimal [symbol] [flags]

Flags:
  -h, --help          help for decimal
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Retrieve all exteranl address by symbol and address type 
```
$docker-compose run bhcli query address addresses -h
addresses symbol address_type

Usage:
  bhcli query address addresses [symbol] [address_type] [flags]

Flags:
  -h, --help          help for addresses
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Retrieve an address's asset specified symbol and address type
```
$docker-compose run bhcli query address address_asset -h
address_asset symbol address_type address

Usage:
  bhcli query address address_asset [symbol] [address_type] [address] [flags]

Flags:
  -h, --help          help for address_asset
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### List all distribution query commands 
```
$docker-compose run bhcli query distr -h
Querying commands for the distribution module

Usage:
  bhcli query distr [command]

Available Commands:
  params                        Query distribution params
  validator-outstanding-rewards Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations
  commission                    Query distribution validator commission
  slashes                       Query distribution validator slashes
  rewards                       Query all distribution delegator rewards or rewards from a particular validator
  community-pool                Query the amount of coins in the community pool

Flags:
  -h, --help   help for distr

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Retrieve distribution's parameters 
```
$docker-compose run bhcli query distr params -h
Query distribution params

Usage:
  bhcli query distr params [flags]

Flags:
  -h, --help          help for params
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

#### Retrieve a validtor un-withdrawn rewards for itself and it's delegators 
```
$docker-compose run bhcli query distr validator-outstanding-rewards -h
Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations

Usage:
  bhcli query distr validator-outstanding-rewards [] [flags]

Flags:
  -h, --help          help for validator-outstanding-rewards
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### Retrieve a validator's commission 
```
$docker-compose run bhcli query distr commission -h
Query validator commission rewards from delegators to that validator:

$ gaiacli query distr commission cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  bhcli query distr commission [validator] [flags]

Flags:
  -h, --help          help for commission
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### Search the slash records between given block range 
```
$docker-compose run bhcli query distr slashes -h
Query all slashes of a validator for a given block range:

$ gaiacli query distr slashes cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj 0 100

Usage:
  bhcli query distr slashes [validator] [start-height] [end-height] [flags]

Flags:
  -h, --help          help for slashes
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### Query all rewads earned by a delegator, or rewards from specified validtor 
```
$docker-compose run bhcli query distr rewards -h
Query all rewards earned by a delegator, optionally restrict to rewards from a single validator:

$ gaiacli query distr rewards cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p
$ gaiacli query distr rewards cosmos1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj

Usage:
  bhcli query distr rewards [delegator-addr] [<validator-addr>] [flags]

Flags:
  -h, --help          help for rewards
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
#### Query all coins in the community pool
```
$docker-compose run bhcli query distr community-pool -h
Query all coins in the community pool which is under Governance control.

$ gaiacli query distr community-pool

Usage:
  bhcli query distr community-pool [flags]

Flags:
  -h, --help          help for community-pool
      --indent        Add indent to JSON response
      --ledger        Use a connected Ledger device
      --node string   <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/Users/rogan/.bhcli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```

