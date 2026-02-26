# Services Management

Check logs

```bash
sudo journalctl -u republicd -f
```

Sync info

```bash
republicd status 2>&1 | jq .sync_info
```

Node info

```bash
republicd status 2>&1 | jq .node_info
```

Your node peer

```bash
echo $(republicd comet show-node-id)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.republic/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

Restart Nodes

<pre><code><strong>sudo systemctl restart republicd &#x26;&#x26; sudo journalctl -u republicd -f
</strong></code></pre>
