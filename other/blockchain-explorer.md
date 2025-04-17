# Blockchain Explorer

### Ping.Pub Explorer (Cosmos SDK Based Chain)

**Prerequisites**\
install nodeJS and Yarn first, check [https://documentation.codeblocklabs.com/other/nodejs#install-nodejs](nodejs.md)

<pre><code>// Running with yarn
yarn --ignore-engines &#x26;&#x26; yarn serve


// Building for web servers, like nginx, apache
yarn --ignore-engines &#x26;&#x26; yarn build
cp -r ./dist/* &#x3C;ROOT_OF_WEB_SERVER>

// Running with docker, change the port 8088 to any port you want.
./docker.sh
<strong>docker run -d -p 8088:80 ping.pub/dashboard
</strong></code></pre>

