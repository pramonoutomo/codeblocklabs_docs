# Key Management

## Key Management

### Add New Wallet

```bash
qied keys add wallet --home $HOME/.qie
```

### Restore Existing Wallet

```bash
qied keys add wallet --recover --home $HOME/.qie
```

### List All Wallets

```bash
qied keys list --home $HOME/.qie
```

### Delete Wallet

```bash
qied keys delete wallet --home $HOME/.qie
```

### Check Balance

```bash
qied q bank balances $(qied keys show wallet -a --home $HOME/.qie) --home $HOME/.qie
```

### Export Key

```bash
qied keys export wallet --home $HOME/.qie
```

### Export Key to EVM

```bash
qied keys export wallet --unarmored-hex --unsafe --home $HOME/.qie
```

### Import Key

```bash
qied keys import wallet wallet.backup --home $HOME/.qie
```
