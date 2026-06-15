# Governance

Create New Text Proposal

```bash
jaynd  tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000arai \
--type Text \
--from wallet \
--chain-id thejaynetwork \
--gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay \
-y 
```

Proposals List

```bash
jaynd query gov proposals
```

View proposal

```bash
jaynd query gov proposal 1
```

Vote Yes

```bash
jaynd tx gov vote 1 yes --from wallet --chain-id thejaynetwork  --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Vote No

```bash
jaynd tx gov vote 1 no --from wallet --chain-id thejaynetwork  --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Vote No With Veto

```bash
jaynd tx gov vote 1 no_with_veto --from wallet --chain-id thejaynetwork  --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Vote Abstain

```bash
jaynd tx gov vote 1 abstain --from wallet --chain-id thejaynetwork  --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```
