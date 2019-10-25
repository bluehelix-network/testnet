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

TBD
