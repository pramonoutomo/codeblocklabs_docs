# Snapshot

```plaintext
sudo apt install lz4 -y
```

```plaintext
sudo systemctl stop republicd
```

```plaintext
cp $HOME/.republic/data/priv_validator_state.json $HOME/.republic/priv_validator_state.json.backup
```

```plaintext
republicd comet unsafe-reset-all --home $HOME/.republic --keep-addr-book
```

{% tabs %}
{% tab title="Republic Team" %}
```
wget https://republic-snapshots.s3.amazonaws.com/republic-snapshot-20260226.tar.gz
tar -xzf republic-snapshot-20260226.tar.gz -C ~/.republic/
```
{% endtab %}

{% tab title="CodeBlockLabs" %}
```plaintext
curl -L http://45.76.1.67/republic/latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.republic
```
{% endtab %}
{% endtabs %}

```plaintext
mv $HOME/.republic/priv_validator_state.json.backup $HOME/.republic/data/priv_validator_state.json
```

```plaintext
sudo systemctl restart republicd
sudo journalctl -u republicd -f -o cat
```
