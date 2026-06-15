# Services Management

Check logs

```bash
sudo journalctl -u jaynd -f
```

Sync info

```bash
jaynd status 2>&1 | jq .sync_info
```

Node info

```bash
jaynd status 2>&1 | jq .node_info
```

Your node peer

```bash
echo $(jaynd comet show-node-id)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.jayn/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

Restart Nodes

<pre><code><strong>sudo systemctl restart jaynd &#x26;&#x26; sudo journalctl -u jaynd -f
</strong></code></pre>
