# NodeJS

Error Installing NodeJS?

```
// install NodeJS version 18
sudo apt remove nodejs  
sudo apt remove nodejs-doc
sudo dpkg --remove --force-remove-reinstreq libnode-dev
sudo dpkg --remove --force-remove-reinstreq libnode72:amd64
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt-get install -y nodejs
```

{% hint style="info" %}
copy and run each line one by one to minimalize the error you could found.
{% endhint %}

