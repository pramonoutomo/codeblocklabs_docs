# Governance

## Governance

### Create New Text Proposal

```bash
qied tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000aqie \
--type Text \
--from wallet \
--chain-id qie_1990-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie \
--home $HOME/.qie \
-y
```

### Proposals List

```bash
qied query gov proposals --home $HOME/.qie
```

### View Proposal

```bash
qied query gov proposal 1 --home $HOME/.qie
```

### Vote Yes

```bash
qied tx gov vote 1 yes --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Vote No

```bash
qied tx gov vote 1 no --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Vote No With Veto

```bash
qied tx gov vote 1 no_with_veto --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Vote Abstain

```bash
qied tx gov vote 1 abstain --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```
