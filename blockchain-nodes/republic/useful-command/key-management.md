# Key Management

Add New Wallet

```bash
republicd keys add wallet
```

Restore executing wallet

```bash
republicd keys add wallet --recover
```

List All Wallets

```bash
republicd keys list
```

Delete wallet

```bash
republicd keys delete wallet
```

Check Balance

```bash
republicd q bank balances $(republicd keys show wallet -a)
```

Export Key (save to wallet.backup)

```bash
republicd keys export wallet
```

Export Key to EVM

```bash
republicd keys export wallet --unarmored-hex --unsafe
```

Import Key (restore from wallet.backup)

```bash
republicd keys import wallet wallet.backup
```
