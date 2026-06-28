## Nmap Quick Reference
```bash
-- Basic scan
nmap -sV -sC -p- IP
-- DC discovery
nmap -p 88,389,445,3268 192.168.10.0/24
-- Script scan
nmap --script=ldap* IP
-- OS detection
nmap -O IP
```
