# Chain Replicator

## Overview

This script is designed to manipulate a Cosmos-SDK genesis export file, specifically modifying validator information and other parameters to facilitate the initialization of a local testnet based on mainnet state. The script makes the following modifications:

1. Removes smart contract codes, contracts, and sequences.
2. Sets all validators to jailed, so you can run the chain with 1 validator only
3. Replaces the mainnet public key of the a desired validator with a new local public key.
4. Updates the genesis file with the necessary changes to enable the validator and shorten the governance parameters to make testing of upgrades easier.

## Usage

## Configuration

Modify the folling variables in the `create_genesis.py` script to suit your needs:

```python
    kintsugi_addr = "junovaloper1juczud9nep06t0khghvm643hf9usw45r23gsmr"
    kintsugi_byte_addr = "31E927F677282369B7E57D39FF9C47E3845BFDEA"
    kintsugi_conskey = "junovalcons1x85j0anh9q3kndl905ull8z8uwz9hl02y0n499"

    replace_pubkey = "fscxRe/wWtcPp07H4WfiH89kQYEYBYTjRJi2TbhX+Lk="
    replace_byte_addr = "0FAF0893503B86A775C12F434BBC17EAD232C03F"
    replace_conskey = "junovalcons1p7hs3y6s8wr2wawp9ap5h0qhatfr9spl8qyt2s"
```

You can create replacement keys by looking inside "priv_validator_key.json" file of a newly initialized local node and using the command `junod tendermint show-address`

## Create new gensis

Export state from mainnet, and generate a new gensis file

```bash
# from the chain you want to replicate, export the state to a json file
junod export --for-zero-height --output-document genesis_export.json

# manipulate the export file to create a brand new genesis that can be used to start a new chain based on mainnet state
python3 create_genesis.py
```

Now you can copy paste `gensis_new.json` to your local node ~/.juno/config/genesis.json. If needed run `junod comet unsafe-reset-all` to reset the chain before starting it.
