---
description: Host your very own blockchain explorer
---

# Blockchain Explorer

### Ping.Pub Explorer (Cosmos SDK Based Chain)

**Prerequisites**\
install nodeJS and Yarn first, check [https://documentation.codeblocklabs.com/other/nodejs#install-nodejs](nodejs.md)

Fork the [official ping.pub](https://github.com/ping-pub/explorer) ([https://github.com/ping-pub/explorer](https://github.com/ping-pub/explorer)) so you can use only chain you wanted to be on your blockchain explorer.&#x20;

<pre><code>// clone your ping.pub explorer from your own repository
git clone https://github.com/YourUsername/explorer.git
// go to the folder
cd explorer
// create new screen
screen -S explorer

// Running with yarn
yarn --ignore-engines &#x26;&#x26; yarn serve

// Building for web servers, like nginx, apache
yarn --ignore-engines &#x26;&#x26; yarn build
cp -r ./dist/* &#x3C;ROOT_OF_WEB_SERVER>

// Running with docker, change the port 8088 to any port you want.
./docker.sh
<strong>docker run -d -p 8088:80 ping.pub/dashboard
</strong></code></pre>

Update

To Update your explorer, if you made any changes to your own repository.&#x20;

```
// open explorer screen
screen -r explorer
// Stop your explorer first, if it's using yarn, simply CTRL+C on the screen
// Pull the update
git pull
// restart the explorer
yarn --ignore-engines && yarn serve

```
