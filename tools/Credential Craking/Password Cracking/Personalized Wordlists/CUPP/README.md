# [CUPP](https://github.com/Mebus/cupp)
***
### CUPP is a tool used to create a custom wordlist based on given information.

#### Installation
```shell
$ sudo apt install cupp
```

#### Interactive mode
```shell
$ cupp -i
```

### Password Policy
#### Common policies:
1. 8 characters or longer
2. contains special characters
3. contains numbers

#### Some tools would convert password policies to **Hashcat** or **John** rules, but **Hydra** does  not support rules for filtering passwords.
#### Commands to remove passwords based on  a given policy
```bash
sed -ri '/^.{,7}$/d' william.txt            # remove shorter than 8
sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt # remove no special chars
sed -ri '/[0-9]+/!d' william.txt            # remove no numbers
```
