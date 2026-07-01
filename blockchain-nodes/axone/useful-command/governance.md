# Governance

## Governance

### Create New Text Proposal

```bash
limonatad tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000alimo \
--type Text \
--from wallet \
--chain-id limonata_10777-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo \
--home $HOME/.limonata \
-y
```

### Proposals List

```bash
limonatad query gov proposals --home $HOME/.limonata
```

### View Proposal

```bash
limonatad query gov proposal 1 --home $HOME/.limonata
```

### Vote Yes

```bash
limonatad tx gov vote 1 yes --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Vote No

```bash
limonatad tx gov vote 1 no --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Vote No With Veto

```bash
limonatad tx gov vote 1 no_with_veto --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Vote Abstain

```bash
limonatad tx gov vote 1 abstain --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```
