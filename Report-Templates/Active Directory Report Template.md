## Active Directory Report Example
Title: AS-REP Roasting — Kerberos Hash Extraction
Severity: High
Affected Component:
testlab.local — user account: testuser

Description:

The testuser account has Kerberos preauthentication disabled. Any unauthenticated user can request an AS-REP hash for this account and attempt offline cracking without providing credentials.

Steps to Reproduce:

1 - Run GetNPUsers.py against the domain:

GetNPUsers.py testlab.local/testuser -dc-ip 192.168.10.19 -no-pass -request

2 - AS-REP hash returned
3 - Crack offline with Hashcat:

hashcat -m 18200 hash.txt rockyou.txt

Impact:

Account credentials recovered offline without authentication. Used to pivot into domain and perform further attacks including Kerberoasting and lateral movement to Domain Controller.

Recommendation:

Enable Kerberos preauthentication on all user accounts.
Use strong unique passwords for all domain accounts.
Monitor for unusual AS-REP requests in event logs.
