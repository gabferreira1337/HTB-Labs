# Bashfuscator
***
### Tool to obfuscate bash commands
#### Setup:
```shell
$ git clone https://github.com/Bashfuscator/Bashfuscator
$ cd Bashfuscator
$ pip3 install setuptools==65
$ python3 setup.py install --user
```

#### Simple examples:
```shell
$ ./bashfuscator -c 'cat /etc/passwd'
```

```shell
$  ./bashfuscator -c 'cat /etc/passwd' -s 1 -t 1 --no-mangling --layers 1
```

#### And then test the output
```shell
$ bash -c 'eval "$(W0=(w \  t e c p s a \/ d);for Ll in 4 7 2 1 8 3 2 4 8 5 7 6 6 0 9;{ printf %s "${W0[$Ll]}";};)"'
```


### Useful flags:
* `-c` =  Specify which command to obfuscate
* `-s` =  Control added size (from 1-3)
* `-t` =  Control added execution time (from 1-3)
* `--layers` = Control amount of obfuscation layers  (default 2)
* `--test` = test generated payload
* `--choose-mutators` = fine-tune obfuscation process