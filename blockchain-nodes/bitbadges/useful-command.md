# Useful Command

### 1. Key Management

#### Create a New Wallet

```bash
bashbitbadgeschaind keys add $BITBADGES_WALLET
```

> Keep the generated seed phrase safe. It is the only way to recover your wallet.

#### Recover Wallet from Seed Phrase

```bash
bashbitbadgeschaind keys add $BITBADGES_WALLET --recover
```

> You will be prompted to enter your seed phrase.

#### List Wallets

```bash
bashbitbadgeschaind keys list
```

#### Show Wallet Details

```bash
bashbitbadgeschaind keys show $BITBADGES_WALLET
```

#### Delete Wallet

```bash
bashbitbadgeschaind keys delete $BITBADGES_WALLET
```

### 2. Wallet Operations

#### Check Balance

```bash
bashbitbadgeschaind query bank balances $(bitbadgeschaind keys show $BITBADGES_WALLET -a)
```

#### Send Tokens

```bash
bashbitbadgeschaind tx bank send $(bitbadgeschaind keys show $BITBADGES_WALLET -a) <receiver_wallet_address> <amount><denom> \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
-y
```

> Example:

```bash
bashbitbadgeschaind tx bank send $(bitbadgeschaind keys show $BITBADGES_WALLET -a) bb...yyy 1000000ubadge \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
-y
```

### 3. Validator Management

Ensure you have enough `ubadge` tokens for self-delegation and transaction fees before creating a validator.

#### Create Validator

```bash
bashbitbadgeschaind tx staking create-validator \
--amount 1000000000ubadge \
--from $BITBADGES_WALLET \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(bitbadgeschaind comet show-validator) \
--moniker "YourMoniker" \
--identity "KeybaseKey" \
--website "YourWebsite.com" \
--details "YourDescription" \
--security-contact "youremail@example.com" \
--chain-id $BITBADGES_CHAIN_ID \
--gas auto --gas-adjustment 1.4 --fees 400ubadge \
-y
```

#### Create Validator with json

```bash
bashcd $HOME
# Create validator.json file
echo "{\"pubkey\":{\"@type\":\"/cosmos.crypto.ed25519.PubKey\",\"key\":\"$(bitbadgeschaind comet show-validator | grep -Po '\"key\":\s*\"\K[^"]*')\"},
    \"amount\": \"1000000000000000000ubadge\",
    \"moniker\": \"YourMoniker\",
    \"identity\": \"KeybaseKey\",
    \"website\": \"YourWebsite.com\",
    \"security\": \"youremail@example.com\",
    \"details\": \"YourDescription\",
    \"commission-rate\": \"0.1\",
    \"commission-max-rate\": \"0.2\",
    \"commission-max-change-rate\": \"0.01\",
    \"min-self-delegation\": \"1\"
}" > $HOME/.bitbadgeschain/validator.json

# Create validator
bitbadgeschaind tx staking create-validator $HOME/.bitbadgeschain/validator.json \
--from $BITBADGES_WALLET --chain-id $BITBADGES_CHAIN_ID \
--gas-adjustment 1.4 --gas auto --gas-prices 400ubadge \
-y
```

#### Delegate Tokens

```bash
bashbitbadgeschaind tx staking delegate $(bitbadgeschaind keys show $BITBADGES_WALLET --bech val -a) <amount>ubadge \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

#### Withdraw Rewards (Delegator)

```bash
bashbitbadgeschaind tx distribution withdraw-rewards $(bitbadgeschaind keys show $BITBADGES_WALLET --bech val -a) \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

#### Withdraw Rewards (Validator Commission)

```bash
bashbitbadgeschaind tx distribution withdraw-rewards $(bitbadgeschaind keys show $BITBADGES_WALLET --bech val -a) --commission \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

#### Unbond Tokens

```bash
bashbitbadgeschaind tx staking unbond $(bitbadgeschaind keys show $BITBADGES_WALLET --bech val -a) <amount>ubadge \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

#### Redelegate Tokens

```bash
bashbitbadgeschaind tx staking redelegate $(bitbadgeschaind keys show $BITBADGES_WALLET --bech val -a) <destination_validator_address> <amount>ubadge \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

#### Jail Info

```bash
bashbitbadgeschaind q slashing signing-info $(bitbadgeschaind tendermint show-validator) 
```

#### Unjail Validator

```bash
bashbitbadgeschaind tx slashing unjail \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

### 4. Node Status & Info

#### Check Sync Status

```bash
bashbitbadgeschaind status 2>&1 | jq .SyncInfo
```

#### Check Peer Info

```bash
bashbitbadgeschaind status 2>&1 | jq .SyncInfo.catching_up
bitbadgeschaind status 2>&1 | jq .NodeInfo.listen_addr
```

#### Show Node ID

```bash
bashbitbadgeschaind tendermint show-node-id
```

#### Restart Node

```bash
bashsudo systemctl restart bitbadgeschaind
```

#### View Node Logs

```bash
bashsudo journalctl -u bitbadgeschaind -fo cat
```

### 5. Governance

#### List Proposals

```bash
bashbitbadgeschaind query gov proposals
```

#### View Proposal Details

```bash
bashbitbadgeschaind query gov proposal <proposal_id>
```

#### Vote on Proposal

```bash
bashbitbadgeschaind tx gov vote <proposal_id> <yes|no|no_with_veto|abstain> \
--chain-id $BITBADGES_CHAIN_ID \
--gas-prices 0.025ubadge \
--gas-adjustment 1.5 \
--gas auto \
--from $BITBADGES_WALLET \
-y
```

### 6. Environment Variables

Make sure to set these in your `~/.bash_profile` or equivalent shell config:

```bash
bashexport BITBADGES_WALLET="wallet"
export BITBADGES_MONIKER="YourMoniker"
export BITBADGES_CHAIN_ID="bitbadges-1"
export BITBADGES_PORT="13"
```

Apply the changes with:

```bash
bashsource ~/.bash_profile
```

***

This cheat sheet is provided to streamline Bitbadges Chain validator and wallet management.
