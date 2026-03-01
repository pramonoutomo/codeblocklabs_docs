# Increase Disk

For Example increase disk of VM 101 to 100G

<pre><code><strong>// run this on proxmox host
</strong><strong>qm resize 101 scsi0 100G
</strong></code></pre>

Resize the disk on VM

```
// Run this on VM
#!/bin/bash

set -e

echo "=== Install tools ==="
sudo apt update -y
sudo apt install -y cloud-guest-utils lvm2

echo "=== Resize partition sda3 ==="
sudo growpart /dev/sda 3

echo "=== Resize Physical Volume ==="
sudo pvresize /dev/sda3

echo "=== Extend Logical Volume ==="
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv

echo "=== Resize Filesystem ==="
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv

echo "=== DONE ==="
df -h
lsblk
```

Verify new disk

```
// run this on VM
df -h
```
