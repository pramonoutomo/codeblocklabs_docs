# Key Management

## Key Management

### Add New Wallet

```bash
pchaind keys add wallet --home $HOME/.pushchain
```

### Restore Existing Wallet

```bash
pchaind keys add wallet --recover --home $HOME/.pushchain
```

### List All Wallets

```bash
pchaind keys list --home $HOME/.pushchain
```

### Delete Wallet

```bash
pchaind keys delete wallet --home $HOME/.pushchain
```

### Check Balance

```bash
pchaind q bank balances $(pchaind keys show wallet -a --home $HOME/.pushchain) --home $HOME/.pushchain
```

### Export Key

```bash
pchaind keys export wallet --home $HOME/.pushchain
```

### Export Key to EVM

```bash
pchaind keys export wallet --unarmored-hex --unsafe --home $HOME/.pushchain
```

### Import Key

```bash
pchaind keys import wallet wallet.backup --home $HOME/.pushchain
```
