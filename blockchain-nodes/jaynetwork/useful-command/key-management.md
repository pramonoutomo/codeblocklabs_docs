# Key Management

Add New Wallet

```bash
jaynd keys add wallet
```

Restore executing wallet

```bash
jaynd keys add wallet --recover
```

List All Wallets

```bash
jaynd keys list
```

Delete wallet

```bash
jaynd keys delete wallet
```

Check Balance

```bash
jaynd q bank balances $(jaynd keys show wallet -a)
```

Export Key (save to wallet.backup)

```bash
jaynd keys export wallet
```

Export Key to EVM

```bash
jaynd keys export wallet --unarmored-hex --unsafe
```

Import Key (restore from wallet.backup)

```bash
jaynd keys import wallet wallet.backup
```
