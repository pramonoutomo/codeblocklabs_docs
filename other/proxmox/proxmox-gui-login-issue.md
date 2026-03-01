# Proxmox GUI Login Issue

Set password default untuk GUI Proxmox (biasanya issue untuk yang login ke proxmox host pakai keychain, karena tidak ada password defaultnya)

## Set Root Password

Login sebagai root (kamu sudah login via SSH key), lalu jalankan:

```
passwd
```

Masukkan password baru → ulangi konfirmasi.

Selesai ✅

Sekarang password itu dipakai untuk login ke:

```
https://IP_SERVER:8006
```

User:

```
root
```

Realm:

```
Linux PAM standard authentication
```

Password:\
→ yang baru kamu buat
