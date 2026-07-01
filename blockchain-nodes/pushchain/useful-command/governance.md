# Governance

## Governance

### Create New Text Proposal

```bash
pchaind tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000upc \
--type Text \
--from wallet \
--chain-id push_42101-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25upc \
--home $HOME/.pushchain \
-y
```

### Proposals List

```bash
pchaind query gov proposals --home $HOME/.pushchain
```

### View Proposal

```bash
pchaind query gov proposal 1 --home $HOME/.pushchain
```

### Vote Yes

```bash
pchaind tx gov vote 1 yes --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Vote No

```bash
pchaind tx gov vote 1 no --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Vote No With Veto

```bash
pchaind tx gov vote 1 no_with_veto --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Vote Abstain

```bash
pchaind tx gov vote 1 abstain --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```
