# GLIBC Problem & Solution

Some nodes binary requires GLIBC 2.38+, but Ubuntu 22.04 only has GLIBC 2.35. You don't need to reinstall your machine for running the binary, you can patch it and make it running with newer GLIBC version.

The error found will like:

```
binaryd: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.38' not found
```

\
We will use **patchelf** to make the binary use a separate GLIBC 2.39 library without touching the system GLIBC. This keeps your system and other applications completely safe.

***

#### Install Dependencies

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git jq patchelf
```

#### Install GLIBC 2.39

```
# Download pre-built GLIBC 2.39
cd $HOME
wget -O glibc-2.39-ubuntu24.tar.gz https://github.com/pramonoutomo/GLIBC/raw/refs/heads/main/glibc-2.39-ubuntu24.tar.gz

# Extract
tar -xzvf glibc-2.39-ubuntu24.tar.gz

# Move to /opt (isolated location)
sudo mkdir -p /opt/glibc-2.39/lib
sudo mv glibc-transfer/* /opt/glibc-2.39/lib/

# Verify
/opt/glibc-2.39/lib/ld-linux-x86-64.so.2 --version

# Cleanup
rm -rf glibc-transfer glibc-2.39-ubuntu24.tar.gz
```

You should see: `ld.so (Ubuntu GLIBC 2.39...) stable release version 2.39`

#### Patch The Binary

```
# Download binary
cd $HOME
VERSION="v0.1.0"
curl -L "https://somewebsite.com/binaryd-linux-amd64" -o binaryd
chmod +x binaryd

# Patch binary to use GLIBC 2.39
patchelf --set-interpreter /opt/glibc-2.39/lib/ld-linux-x86-64.so.2 binaryd
patchelf --set-rpath /opt/glibc-2.39/lib binaryd

# Move to bin
sudo mv binaryd /usr/local/bin/binaryd

# Test - should show version without GLIBC error
binaryd version
```

GLIBC Error Still Appears?

```
# Verify patchelf settings
patchelf --print-interpreter /usr/local/bin/binaryd
patchelf --print-rpath /usr/local/bin/binaryd

# Should show:
# /opt/glibc-2.39/lib/ld-linux-x86-64.so.2
# /opt/glibc-2.39/lib
```

\
\
Reference:\
[Coinsspor Github](https://github.com/coinsspor/Republic-AI-Testnet-Node-Installation-Guide-Ubuntu-22.04-Compatible/blob/main/README.md)
