## FFUF

***
### FFUF is a great tool for web fuzzing (***Fuzzing directories, files and extensions, PHP parameters, parameter values, identifying hidden VHosts, etc.***)

***
#### **Fuzzing** is a testing method that sends various types of user input to a certain interface.

### Wordlists
* Seclists 
  * Pages and directory fuzzing wordlist (directory-list-2.3)

### Directory Fuzzing Example:
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://<SERVER_IP>:<PORT>/FUZZ
```

### Extension Fuzzing Example:
#### Examples of extensions: .html, .aspx or .asp (most common for IIS servers) , .php (most common for apache servers)
#### Extension Only Fuzzing:
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://<SERVER_IP>:<PORT>/indexFUZZ
```
#### Extension Only Fuzzing:
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://<SERVER_IP>:<PORT>/indexFUZZ
```

#### After detecting which extensions the website is using lets fuzz for page fuzzing

### Page Fuzzing Example using .php extension
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://<SERVER_IP>:<PORT>/1337/FUZZ.php
```

### Recursive Fuzzing Example:
#### Using the flag -recursion it's possible to enable recursive scanning and consequently specify the depth using the **-recursion-depth**flag. For example, if we use a recursion of depth 1 , it will only fuzz the main directories plus their sub-directories
#### When using recursion, we can use the -e flag to specify our extension. For example: -e .html

```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://<SERVER_IP>:<PORT>/FUZZ -recursion -recursion-depth 1 -e .php
```

### Sub-domain Fuzzing (*.website.com) Example:
#### After the recursive scan we can perform a sub-domain fuzzing  to see if we find anything 
#### The FFUF tool will only detect public sub-domains , so even if it does not return anything it doesn't mean that there are no sub-domain
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://FUZZ.website.com/
```

### VHost Fuzzing Example:
#### When we want to fuzz sub-domains that do not have a public DNS record or sub-domains under websites that are not public, we would try VHost fuzzing
#### A VHost is a "sub-domain" served on the same server and has the same IP, so a single IP could be serving 2 or more different websites
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://website.com/ -H "Host: FUZZ.website.com"
```
### Parameter Fuzzing - Get Example:
```Bash
$ ffuf -w /opt/useful/SecLists/Discovery/web-Content/burp-parameter-names.txt -u http://website.com/admin/admin.php?FUZZ=key -fs xxx
```

### Parameter Fuzzing - Post Example:
```Bash
$ ffuf -X POST -w /opt/useful/SecLists/Discovery/web-Content/burp-parameter-names.txt -u http://website.com/admin/admin.php -d "FUZZ=key" -H "Content-Type: application/x-www-form-urlencoded" -fs xxx
```
#### Note: In PHP, "POST" data "content-type" can only accept  "application/x-www-form-urlencoded"

### Value Fuzzing:
#### When we are done with parameter fuzzing , we can try to fuzz the value
#### Simple Bash script to generate values from a value to another and store it in a txt file:
```Bash
$ for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
```Bash
$ ffuf -X POST -w /opt/useful/SecLists/Discovery/web-Content/burp-parameter-names.txt -u http://website.com/admin/admin.php -d "id=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -fs xxx
```
#### Note: In PHP, "POST" data "content-type" can only accept  "application/x-www-form-urlencoded"


### MATCHER OPTIONS Flags:
*  -mc      :           Match HTTP status codes, or "all" for everything. (default: 200-299,301,302,307,401,403,405,500)
*  -ml         :        Match amount of lines in response
*  -mmode    :          Matcher set operator. Either of: and, or (default: or)
*  -mr       :          Match regexp
*  -ms       :          Match HTTP response size
*  -mt        :         Match how many milliseconds to the first response byte, either greater or less than. EG: >100 or <100
*  -mw       :          Match amount of words in response

### FILTER OPTIONS Flags:
* -fc        :         Filter HTTP status codes from response. Comma separated list of codes and ranges
* -fl         :        Filter by amount of lines in response. Comma separated list of line counts and ranges
*  -fmode      :        Filter set operator. Either of: and, or (default: or)
*  -fr         :        Filter regexp
*  -fs         :        Filter HTTP response size. Comma separated list of sizes and ranges
*  -ft         :        Filter by number of milliseconds to the first response byte, either greater or less than. EG: >100 or <100
*  -fw        :         Filter by amount of words in response. Comma separated list of word counts and ranges

### Useful Flags
* -w  : wordlists
* -H  : Specify Header
* -u  : URL
* -d  : add data to use in the request
* -X  : Define request type
* -ic : Ignore Comments
* -s  : Silent mode
* -v  : Output full URLs
* -recursion 
* -request  : Use request from file
* -request-proto : Specify protocol to be used
* -recursion-depth
* -e  : Specify extension when using recursion
* --replay-proxy 
* -t  : Number of threads
* -ic : Ignore Comments
* -h  : Help
