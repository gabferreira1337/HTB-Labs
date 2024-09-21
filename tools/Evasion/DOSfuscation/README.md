# DOSfuscation
***
### Interactive tool to obfuscate OS commands
#### Setup:
```shell
> git clone https://github.com/danielbohannon/Invoke-DOSfuscation.git
> cd Invoke-DOSfuscation
> Import-Module .\Invoke-DOSfuscation.psd1
> Invoke-DOSfuscation
Invoke-DOSfuscation> help
``` 

#### Simple examples:
```shell
Invoke-DOSfuscation> SET COMMAND type C:\Users\student\Desktop\flag.txt
Invoke-DOSfuscation> encoding
Invoke-DOSfuscation\Encoding> 1'
```

#### And then test the output
```shell
yp%TEMP:~-3,-2% %CommonProgramFiles:~17,-11%:\Users\h%TMP:~-13,-12%b-stu%SystemRoot:~-4,-3%ent%TMP:~-19,-18%%ALLUSERSPROFILE:~-4,-3%esktop\flag.%TMP:~-13,-12%xt

test_flag
```


#### **Note** = Through Linux VM it's also possible to run the above tool, using the **pwsh** .