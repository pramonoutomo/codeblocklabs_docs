# Key Management

## Key Management

### Add New Wallet

```bash
latandad keys add wallet --home $HOME/.latanda
```

### Restore Existing Wallet

```bash
latandad keys add wallet --recover --home $HOME/.latanda
```

### List All Wallets

```bash
latandad keys list --home $HOME/.latanda
```

### Delete Wallet

```bash
latandad keys delete wallet --home $HOME/.latanda
```

### Check Balance

```bash
latandad q bank balances $(latandad keys show wallet -a --home $HOME/.latanda) --home $HOME/.latanda
```

### Export Key

```bash
latandad keys export wallet --home $HOME/.latanda
```

### Export Key to EVM

```bash
latandad keys export wallet --unarmored-hex --unsafe --home $HOME/.latanda
```

### Import Key

```bash
latandad keys import wallet wallet.backup --home $HOME/.latanda
```
