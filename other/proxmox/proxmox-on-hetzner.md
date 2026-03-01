# Proxmox on Hetzner

## PHASE 1 — INSTALL DEBIAN (RAID1)

Rescue:

```
installimage
```

Config:

```
DRIVE1 /dev/nvme0n1
DRIVE2 /dev/nvme2n1
#DRIVE3 /dev/nvme1n1

SWRAID 1
SWRAIDLEVEL 1

PART /boot/efi esp 256M
PART /boot ext3 1G
PART lvm vg0 all

LV vg0 root / ext4 100G
LV vg0 swap swap swap 32G
LV vg0 data /var/lib/vz ext4 all
```

Install → reboot.

***

## 🌐 PHASE 2 — NETWORK CONFIG (CRITICAL)

Edit:

```
nano /etc/network/interfaces
```

Isi FINAL:

```
source /etc/network/interfaces.d/*

auto lo
iface lo inet loopback

iface lo inet6 loopback

auto enp5s0
iface enp5s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 65.109.32.61/26
        gateway 65.109.32.1
        bridge-ports enp5s0
        bridge-stp off
        bridge-fd 0
        up route add -net 65.109.32.0 netmask 255.255.255.192 gw 65.109.32.1

iface vmbr0 inet6 static
        address 2a01:4f9:5a:1d8c::2/64
        gateway fe80::1

auto vmbr1
iface vmbr1 inet static
        address 10.10.10.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

        post-up echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -o vmbr0 -j MASQUERADE
        post-down iptables -t nat -D POSTROUTING -s 10.10.10.0/24 -o vmbr0 -j MASQUERADE
```

Reboot:

```
reboot
```

***

## 🖥 PHASE 3 — INSTALL PROXMOX

```
echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list

wget https://enterprise.proxmox.com/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg

apt update
apt full-upgrade -y
apt install proxmox-ve postfix open-iscsi -y
apt remove linux-image-amd64 -y
update-grub
reboot
```

Set root password:

```
passwd
```

Akses:

```
https://HETZNER_IP:8006
```

***

## 💾 PHASE 4 — ZFS STORAGE

```
wipefs -a /dev/nvme1n1
sgdisk --zap-all /dev/nvme1n1

apt install zfsutils-linux -y

zpool create -f -o ashift=12 vmdata /dev/nvme1n1

zfs set compression=lz4 vmdata
zfs set atime=off vmdata
```

GUI → Add ZFS → vmdata.

***

## 🧊 PHASE 5 — DOWNLOAD UBUNTU ISO (FIXED VERSION)

```
cd /var/lib/vz/template/iso
wget https://releases.ubuntu.com/24.04.4/ubuntu-24.04.4-live-server-amd64.iso
```

***

## 🧊 PHASE 6 — TEMPLATE 9000 (ubuntu24-base)

Create VM:

* ID: 9000
* BIOS: OVMF
* Machine: q35
* Disk: 32G (vmdata)
* Add CloudInit (ide2)
* Network: vmbr1
* ISO: ubuntu-24.04.4-live-server-amd64.iso

Install:

* Ubuntu Server (minimized)
* Install OpenSSH
* Continue without network

Login:

```
sudo apt update
sudo apt install qemu-guest-agent cloud-init -y
sudo systemctl enable qemu-guest-agent

sudo systemctl disable systemd-networkd-wait-online.service
sudo systemctl mask systemd-networkd-wait-online.service

sudo truncate -s 0 /etc/machine-id
sudo rm -f /var/lib/dbus/machine-id
sudo cloud-init clean --logs
sudo rm -f /etc/ssh/ssh_host_*
sudo poweroff
```

Cloud-Init tab:

* User: ubuntu
* DNS: 1.1.1.1
* Regenerate Image

Convert → Template.

***

## 🐳 PHASE 7 — DOCKER TEMPLATE (9001)

Clone full dari 9000.

Cloud-Init:

```
ip=10.10.10.11/24,gw=10.10.10.1
DNS=1.1.1.1
```

Install Docker.

Clean → Convert → Template.

***

## 🌌 PHASE 8 — COSMOS TEMPLATE (9002)

Clone full dari 9000.

Cloud-Init:

```
ip=10.10.10.12/24,gw=10.10.10.1
DNS=1.1.1.1
```

Install deps + Go.

Clean → Convert → Template.

***

## 🚀 PHASE 9 — CREATE VM WITH INTERNET

Clone full dari template.

Cloud-Init contoh:

```
ip=10.10.10.20/24,gw=10.10.10.1
DNS=1.1.1.1
```

Bridge: vmbr1

Test:

```
ping 1.1.1.1
ping google.com
```

***

## 🔐 PHASE 10 — SETUP SSH JUMP (TERMIUS)

### STEP A — Add Proxmox Host

Termius → Add Host:

* Address: HETZNER\_IP
* Username: root
* Auth: SSH Key

Test login.

***

### STEP B — Add VM Host (Private)

Termius → Add Host:

* Address: 10.10.10.20
* Username: ubuntu
* Auth: SSH Key
* Proxy / Jump Host: pilih Proxmox host

Save.

***

## 🚀 CONNECT

Klik VM host.

Flow:

```
Laptop → 65.109.32.61 → 10.10.10.20
```

***

## 🔐 OPTIONAL HARDENING

Disable password login:

```
nano /etc/ssh/sshd_config
```

Set:

```
PasswordAuthentication no
PermitRootLogin prohibit-password
```

Restart:

```
systemctl restart ssh
```
