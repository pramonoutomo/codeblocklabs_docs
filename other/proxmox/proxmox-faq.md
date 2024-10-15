# Proxmox FAQ

Q: I can't connect to CT/VM by ssh after install it \
A: check if sshd are running or not by using command:&#x20;
-----------------------------------------------------------

```
// check sshd status
sudo systemctl status sshd
// if not running you can turn it on with command below
sudo systemctl start sshd
```

If you can connect but can't login as root with password, make sure to add permit login

```
// open sshd config
sudo nano /etc/ssh/sshd_config

// Change
// PermitRootLogin without-password
// to
// PermitRootLogin yes
// And then save the file, CTRL+X , Y , Enter
```

***

Q: I can't connect my local machine from outside (i have my public ip setup already to the machine), I want to use 1 ip for all vm/ct.\
A: Use port forwarding to make it possible.
-------------------------------------------

**Enable eth0 route localnet**

{% hint style="danger" %}
replace **eth0** with your own public interface
{% endhint %}

```
sudo sed -i '/net.ipv4.conf.eth0.route_localnet/d' /etc/sysctl.conf
sudo sed -i -e '$anet.ipv4.conf.eth0.route_localnet=1' /etc/sysctl.conf
sudo sysctl -p
```

**Create Firewall Script**

{% hint style="warning" %}
Please change the PUBLICIP and LOCALIP to your own!\
You can also change PUBLICIP to 0.0.0.0/0\
Every port can be forwarded on each line of the scripts.
{% endhint %}

```
#!/bin/bash
/usr/sbin/iptables -t nat -A POSTROUTING -j MASQUERADE
/usr/sbin/iptables -t nat -A PREROUTING -p tcp -d PUBLICIP --dport 10001 -j DNAT --to-destination LOCALIP:10001
/usr/sbin/iptables -t nat -A PREROUTING -p tcp -d PUBLICIP --dport 10002 -j DNAT --to-destination LOCALIP:10002
```

**Create Crontab**

```
crontab -e
```

then add code below

```
@reboot /bin/bash /root/firewall.sh >> /root/firewall.log 2>&1
```

{% hint style="info" %}
this firewall rule will be activated everytime your machine are restarted. also the logs are available on /root/firewall.log if you meet some problem.
{% endhint %}

**Checking Active Rule**

```
sudo iptables -t nat -v -L PREROUTING -n --line-number
```

**Deleting Rule without restarting machine**

```
// check active rule first, then there is a line number of the rule
// for example you want to delete rule number 4 use command below:
sudo iptables -t nat -D PREROUTING 4
// this only delete on this session, if you need to make it permanent, 
// don't forget to remove the rule from firewall.sh 
```

**Add Rule without restarting machine**

```
iptables -t nat -A PREROUTING -p tcp -d PUBLICIP --dport 36657 -j DNAT --to-destination LOCALIP:26657

// this only delete on this session, if you need to make it permanent, 
// don't forget to add the rule from firewall.sh using the 
```

