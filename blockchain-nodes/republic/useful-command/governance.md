# Governance

Create New Text Proposal

```bash
republicd  tx gov submit-proposal \
--title "Governance" \
--description "DETAIL" \
--deposit 1000000arai \
--type Text \
--from wallet \
--gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai \
-y 
```

Proposals List

```bash
republicd query gov proposals
```

View proposal

```bash
republicd query gov proposal 1
```

Vote Yes

```bash
republicd tx gov vote 1 yes --from wallet --chain-id raitestnet_77701-1  --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Vote No

```bash
republicd tx gov vote 1 no --from wallet --chain-id raitestnet_77701-1  --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Vote No With Veto

```bash
republicd tx gov vote 1 no_with_veto --from wallet --chain-id raitestnet_77701-1  --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Vote Abstain

```bash
republicd tx gov vote 1 abstain --from wallet --chain-id raitestnet_77701-1  --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```
