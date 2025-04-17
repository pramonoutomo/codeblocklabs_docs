# NodeJS

### Install NodeJS

<pre><code>// Install Node Version Manager
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash

// Install the latest version of NodeJS
nvm install node # "node" is an alias for the latest version

// Install the latest version of NPM for Node
nvm install-latest-npm # get the latest supported npm version on the current node version

<strong>// Install Yarn
</strong>npm install --global yarn

</code></pre>

***

### Error Installing NodeJS?

```
// error install NodeJS on version 18
sudo apt remove nodejs  
sudo apt remove nodejs-doc
sudo dpkg --remove --force-remove-reinstreq libnode-dev
sudo dpkg --remove --force-remove-reinstreq libnode72:amd64
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt-get install -y nodejs
```

{% hint style="info" %}
copy and run each line one by one to minimalize the error you could found.
{% endhint %}
