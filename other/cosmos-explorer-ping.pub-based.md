# Cosmos Explorer (Ping.Pub Based)

## Quick Install for Prerequisites

1. Install Node Version Manager

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```

2. Install the latest version of NodeJS

```
nvm install node # "node" is an alias for the latest version
```

3. Install the latest version of NPM for Node

```
nvm install-latest-npm # get the latest supported npm version on the current node version
```

4. Install Yarn

```
npm install --global yarn
```

## Install Explorer

```
// Download the source, use ping.pub repo directly, 
// if using my repo, you'll use the same chain as mine :)

// install git
sudo apt-get install git

// download from ping.pub github (choose one, no need to download both)
git pull https://github.com/ping-pub/explorer.git
// or download from our github (choose one, no need to download both)
git pull https://github.com/pramonoutomo/blockchain-explorer.git

// run directly
yarn --ignore-engines && yarn serve
```

Auto Restart with pm2 for easy to use

```
# Quick PM2 setup
cd /root/blockchain-explorer
npm install -g pm2
pm2 start "yarn --ignore-engines && yarn serve --host 0.0.0.0" --name blockchain-explorer
pm2 save

# Enable on startup (run the command PM2 shows you)
pm2 startup
```

Useful pm2 command

```
# View all processes
pm2 list

# Monitor in real-time
pm2 monit

# View logs
pm2 logs blockchain-explorer
pm2 logs blockchain-explorer --lines 100

# Restart the app
pm2 restart blockchain-explorer

# Stop the app
pm2 stop blockchain-explorer

# Delete from PM2
pm2 delete blockchain-explorer

# Reload app on file changes (development)
pm2 start "yarn --ignore-engines && yarn serve --host 0.0.0.0" --name blockchain-explorer --watch
```
