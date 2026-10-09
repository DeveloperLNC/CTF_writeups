# RootMe - THM - Easy Linux Web + Privesc
IP: 10.49.182.150 Date:07/10/2026

## 1. Summary
Web upload bypass to www-data -> SUID python to root. Learned: file upload filter bypass + SUID enum.

## 2. Recon
nmap ports: 22,80. Full scan saved in nmap.txt

## 3. Enumeration
gobuster found: /panel/, /uploads/
Upload at /panel/ blocked .php but allowed .phtml

## 4. Exploitation
Listener: nc -lvnp 9091
Uploaded phtml reverse shell, browsed /uploads/shell.phtml -> shell as www-data
Proof: screenshots/02-shell.png

## 5. PrivEsc
sudo -l: nothing, SUID: /usr/bin/python
GTFOBins python SUID -> root
Proof: screenshots/03-root.png
Flags: user.txt THM{***redacted***}, root.txt THM{***redacted***}

## 6. Fix
- Block executable extensions server-side + validate MIME + store uploads outside webroot
- Remove SUID from python: chmod u-s /usr/bin/python
- Detection: web logs POST to /panel/, Sysmon EventID 1 python spawned sh

## 7. Lesson
Even if it was possible to read the root.txt file by using python with suid enabled and get the CTF solved, investing time on getting right command from GTFOBins for PrivEsc to root was worth as the end goal was getting Root priveleges.
