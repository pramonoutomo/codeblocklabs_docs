# Proxmox Firewall on Hetzner

Pada proxmox host, buat file firewall.sh

```
#!/bin/bash

# Enable IP forward
echo 1 > /proc/sys/net/ipv4/ip_forward

# Flush NAT only (jangan flush filter table)
iptables -t nat -F

# NAT untuk private network
iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -o vmbr0 -j MASQUERADE
# Allow forwarding from VM network
iptables -A FORWARD -s 10.10.10.0/24 -j ACCEPT
iptables -A FORWARD -d 10.10.10.0/24 -j ACCEPT


################################
# Contoh BlockChain A - 10.10.10.101
################################
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 1111 -j DNAT --to 10.10.10.101:1111
iptables -A FORWARD -p tcp -d 10.10.10.101 --dport 1111 -j ACCEPT

################################
# Contoh BlockChain B - 10.10.10.102
################################
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 2222 -j DNAT --to 10.10.10.102:2222
iptables -A FORWARD -p tcp -d 10.10.10.102 --dport 2222 -j ACCEPT
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 2223 -j DNAT --to 10.10.10.102:2223
iptables -A FORWARD -p tcp -d 10.10.10.102 --dport 2223 -j ACCEPT



################################
# KHUSUS DIBAWAH INI UNTUK BISA TERIMA AKSES 80/443 DARI LUAR
# BIASANYA UNTUK NGINX DENGAN VM KHUSUS SEPERTI GATEWAY/NGINX HOST (10.10.10.200)
# SETUP SUB DOMAIN RPC/API DENGAN NGINX + CLOUDFLARE DI VM GATEWAY JUGA PERLU INI
################################
# DNAT only from public interface
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 80 \
  -j DNAT --to-destination 10.10.10.200:80

iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 443 \
  -j DNAT --to-destination 10.10.10.200:443

# Forward allow
iptables -A FORWARD -p tcp -d 10.10.10.200 --dport 80 -j ACCEPT
iptables -A FORWARD -p tcp -d 10.10.10.200 --dport 443 -j ACCEPT

```

Setiap perubahan, wajib melakukan refresh firewallnya dengan command:

```
sudo bash firewall.sh
```

***

Jangan lupa setting CRON, agar setiap restart host semua setting firewall otomatis di load.

```
crontab -e
```

Tambahkan dibagian bawah sendiri:

```
@reboot /bin/bash /root/firewall.sh >> /root/firewall.log 2>&1
```

Save dan selesai.

***

Untuk melihat status yang sedang berjalan apa aja, bisa pakai command:

```
iptables -L -n -v

// or

iptables -t nat -L -n -v
```
