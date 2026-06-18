## Troubleshooting Log
| Problem | Fix |
|---------|-----|
| LDAP error 52e | Wrong password for auth user |
| GetUserSPNs not found | pip3 install impacket --break-system-packages |
| Element not found netsh | Use GUI ncpa.cpl instead |
| No Pwn3d with sqlservice | Switch to Administrator account |
| Server 2012 no internet | Set gateway via ncpa.cpl GUI |

## Defensive Recommendations
- Require Kerberos preauthentication on all accounts
- Use strong unique passwords for service accounts
- Disable WDigest: UseLogonCredential = 0
- Enable Credential Guard
- Implement tiered admin model
- Enable SMB signing
- Monitor for AS-REP and TGS requests in event logs
- Network segmentation — separate DC from workstations
- Principle of least privilege on all service accounts
