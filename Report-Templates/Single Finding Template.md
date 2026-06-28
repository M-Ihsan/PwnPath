# Professional Pentest Report Templates

## Single Finding Template
Title: [Vulnerability Name]

Severity: Critical / High / Medium / Low / Informational

Affected Component: [URL, parameter, service, endpoint]
Description:

[What is the vulnerability in plain English.

Why does it exist. What is affected.]
Steps to Reproduce:

[Exact step with payload]
[Exact step]
[Expected result]

Proof of Concept:

[Screenshot or command output showing vulnerability]
Impact:

[What can an attacker do with this.

Business impact in plain English.]
Recommendation:

[How to fix it. Specific and actionable.]

---

## Severity Rating Guide
| Severity | Example |
|----------|---------|
| Critical | RCE, Domain Admin compromise, SQLi with data dump |
| High | Stored XSS, Auth bypass, Privilege escalation |
| Medium | Reflected XSS, Information disclosure, CSRF |
| Low | Missing headers, robots.txt disclosure |
| Informational | Technology fingerprinting, subdomain enumeration |
