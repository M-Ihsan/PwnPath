## Web Application Report Example

Title: SQL Injection — UNION-Based Credential Extraction

Severity: Critical

Affected Component: http://target.com/vulnerabilities/sqli/?id=

Description:
The id parameter is vulnerable to UNION-based SQL Injection. User input is inserted directly into the SQL query without sanitization, allowing an attacker to modify query logic and extract database contents.

Steps to Reproduce:

1 - Navigate to the SQL Injection page
2 - Enter the following payload in the User ID field: 0 UNION SELECT 1,2,group_concat(username,':',password) FROM users-- 
3 - Submit the form
4 - Database credentials are returned in the response

Impact:

Full database compromise. Attacker can extract all usernames and password hashes, potentially gaining access to admin accounts and sensitive user data.

Recommendation:

Implement prepared statements (parameterised queries).

Never concatenate user input directly into SQL queries.

Example fix in PHP:

$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");

stmt−>execute([stmt->execute([
stmt−>execute([id]);
