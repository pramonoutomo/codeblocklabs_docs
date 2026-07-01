# Governance

## Governance

### Create New Text Proposal

```bash
latandad tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000ultd \
--type Text \
--from wallet \
--chain-id latanda-testnet-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd \
--home $HOME/.latanda \
-y
```

### Proposals List

```bash
latandad query gov proposals --home $HOME/.latanda
```

### View Proposal

```bash
latandad query gov proposal 1 --home $HOME/.latanda
```

### Vote Yes

```bash
latandad tx gov vote 1 yes --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Vote No

```bash
latandad tx gov vote 1 no --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Vote No With Veto

```bash
latandad tx gov vote 1 no_with_veto --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Vote Abstain

```bash
latandad tx gov vote 1 abstain --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```
