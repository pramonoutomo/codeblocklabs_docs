# Key Management

## Key Management

### Add New Wallet

```bash
limonatad keys add wallet --home $HOME/.limonata
```

### Restore Existing Wallet

```bash
limonatad keys add wallet --recover --home $HOME/.limonata
```

### List All Wallets

```bash
limonatad keys list --home $HOME/.limonata
```

### Delete Wallet

```bash
limonatad keys delete wallet --home $HOME/.limonata
```

### Check Balance

```bash
limonatad q bank balances $(limonatad keys show wallet -a --home $HOME/.limonata) --home $HOME/.limonata
```

### Export Key

```bash
limonatad keys export wallet --home $HOME/.limonata
```

### Export Key to EVM

```bash
limonatad keys export wallet --unarmored-hex --unsafe --home $HOME/.limonata
```

### Import Key

```bash
limonatad keys import wallet wallet.backup --home $HOME/.limonata
```
