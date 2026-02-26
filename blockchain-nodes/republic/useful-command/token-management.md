# Token Management

Withdraw all rewards

```bash
republicd tx distribution withdraw-all-rewards --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai
```

Withdraw rewards and commission from your validator

```bash
republicd tx distribution withdraw-rewards $(republicd keys show wallet --bech val -a) --from wallet --commission --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Check your balance

```bash
republicd query bank balances $(republicd keys show wallet -a)
```

Delegate to Yourself

```bash
republicd tx staking delegate $(republicd keys show wallet --bech val -a) 1000000arai --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Delegate to Other Validator

```bash
republicd tx staking delegate VALOPER_ADDRESS 1000000arai --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Redelegate Stake to Another Validator

```bash
republicd tx staking redelegate $(republicd keys show wallet --bech val -a) VALOPER_ADDRESS 1000000arai --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Unbond

```bash
republicd tx staking unbond $(republicd keys show wallet --bech val -a) 1000000arai --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Transfer Funds

```bash
republicd tx bank send $(republicd keys show wallet -a) WALLET_ADDRESS 1000000arai --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

<br>
