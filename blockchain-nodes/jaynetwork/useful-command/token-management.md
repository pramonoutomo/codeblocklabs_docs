# Token Management

Withdraw all rewards

```bash
jaynd tx distribution withdraw-all-rewards --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay
```

Withdraw rewards and commission from your validator

```bash
jaynd tx distribution withdraw-rewards $(jaynd keys show wallet --bech val -a) --from wallet --commission --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Check your balance

```bash
jaynd query bank balances $(jaynd keys show wallet -a)
```

Delegate to Yourself

```bash
jaynd tx staking delegate $(jaynd keys show wallet --bech val -a) 1000000ujay --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Delegate to Other Validator

```bash
jaynd tx staking delegate VALOPER_ADDRESS 1000000ujay --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Redelegate Stake to Another Validator

```bash
jaynd tx staking redelegate $(jaynd keys show wallet --bech val -a) VALOPER_ADDRESS 1000000ujay --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Unbond

```bash
jaynd tx staking unbond $(jaynd keys show wallet --bech val -a) 1000000ujay --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Transfer Funds

```bash
jaynd tx bank send $(jaynd keys show wallet -a) WALLET_ADDRESS 1000000ujay --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

<br>
