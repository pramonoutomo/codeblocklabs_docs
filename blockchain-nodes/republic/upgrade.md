---
description: v0.3.0
---

# Upgrade

{% tabs %}
{% tab title="Cosmovisor" %}
```
cd $HOME
wget https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64  -O republicd
chmod +x republicd
mkdir -p $HOME/.republic/cosmovisor/upgrades/v0.3.0/bin
mv republicd $HOME/.republic/cosmovisor/upgrades/v0.3.0/bin/
sudo systemctl restart republicd && sudo journalctl -u republicd -f
```
{% endtab %}

{% tab title="Non Cosmovisor" %}
```
wget https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64  -O republicd
chmod +x republicd
mv republicd ~/go/bin/
sudo systemctl restart republicd && sudo journalctl -u republicd -f
```
{% endtab %}
{% endtabs %}

