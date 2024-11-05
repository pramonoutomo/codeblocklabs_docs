# Proxmox Installation

What you need to install on local machine?

1. Download Proxmox ISO from [https://www.proxmox.com/en/downloads/proxmox-virtual-environment](https://www.proxmox.com/en/downloads/proxmox-virtual-environment)
2. Prepare a flash drive (8gb is enough for this)
3. [Rufus ](https://rufus.ie/en/) (use it to burn the ISO to your flash drive)
4. Follow the default instruction on the screen
5. Login to https://your\_local\_ip:8006/ with your password submitted on installation GUI.



What you need to install on vds?

1. Install debian (bookworm or any recomended from proxmox ve website). then ssh to the machine.
2. Add the GPG key and adjust the APT sources:

```
curl -o /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg http://download.proxmox.com/debian/proxmox-release-bookworm.gpg
echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
```

If you do not have a Proxmox VE Enterprise subscription and wish to avoid unauthorized access errors, you can comment out the Proxmox VE Enterprise repository with the following command:

```go
echo '# deb https://enterprise.proxmox.com/debian/pve bookworm InRelease' > /etc/apt/sources.list.d/pve-enterprise.list
```

Update the packages:

```go
apt update        # Update package lists
apt full-upgrade  # Update system
```

3.  Install Proxmox VE

    ```go
    apt install proxmox-ve
    ```

    Restart the server

    After a restart, the Proxmox kernel should be loaded. Run the following command in order to see kernel release information:

    ```go
    uname -r
    ```

    The output should contain `pve`, for example: `6.5.13-1-pve`

    You can access the web interface by navigating to `https://<main address>:8006`.
4. Enable IP Forwarding on the Host

*   For IPv4 and IPv6:

    ```bash
    sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
    sed -i 's/#net.ipv6.conf.all.forwarding=1/net.ipv6.conf.all.forwarding=1/' /etc/sysctl.conf
    ```
*   Apply the changes:

    ```bash
    sysctl -p
    ```
*   Check if the forwarding is active:

    ```bash
    sysctl net.ipv4.ip_forward
    sysctl net.ipv6.conf.all.forwarding
    ```

5. Setup Network Configuration on Host
   1.  example of host network\


       <figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
   2.  example of client network\


       <figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
6. Setup VM Templates
   1. go to CT Templates Menu\
      ![](<../../.gitbook/assets/image (5).png>)
   2.  select template you want to download and add to your proxmox, you can choose any operating system ISO from here.\


       <figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
7. If your VM/CT can't connect to internet, use post routing command, make sure to chane the IP to your own local ip settings before.

```
iptables -t nat -A POSTROUTING -s 192.168.100.0/24 -o vmbr0 -j MASQUERADE
```

8. add your forward port and setting ip automatically by using cron&#x20;

create new file firewall.sh

```
#!/bin/bash
/usr/sbin/iptables -t nat -A POSTROUTING -j MASQUERADE
/usr/sbin/iptables -t nat -A PREROUTING -p tcp -d 0.0.0.0/0 --dport 20002 -j DNAT --to-destination 192.168.110.150:20002
/usr/sbin/iptables -t nat -A PREROUTING -p tcp -d 0.0.0.0/0 --dport 20620 -j DNAT --to-destination 192.168.110.151:20620
/usr/sbin/iptables -t nat -A PREROUTING -p tcp -d 0.0.0.0/0 --dport 20619 -j DNAT --to-destination 192.168.110.151:20619
```

{% hint style="info" %}
you can port forward every port accessing your IP to your own local ip:port directly.
{% endhint %}

add the file to the cron, use command `crontab -e`  , if you're first time opening crontab, you'll be asked to choose which editor you want to use, default and easiest one is nano.

```
@reboot /bin/bash /root/firewall.sh >> /root/firewall.log 2>&1
```

adding command above are making the network forward rule to be reloaded everytime restart/reboot happens to the machine.
