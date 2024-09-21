# Hydra Modules
***
###
#### To cause minimal network traffic , it is better to try the top 10 most popular admins credentials.
#### If we didn't get any access, we should then use `password spraying`. By reusing passwords .

### Brute Forcing Forums
#### Types of requests to use to bruteforce services:
```bash
hydra -h | grep "Supported services" | tr ":" "\n" | tr " " "\n" | column -e

Supported			        ldap3[-{cram|digest}md5][s]	rsh
services			        memcached					rtsp
				            mongodb						s7-300
adam6500			        mssql						sip
asterisk			        mysql						smb
cisco				        nntp						smtp[s]
cisco-enable		        oracle-listener				smtp-enum
cvs				            oracle-sid					snmp
firebird			        pcanywhere					socks5
ftp[s]				        pcnfs						ssh
```
#### The `http[s]-{head|get|post}` module serves for basic HTTP, while the `http[s]-post-form` module is used for login forms, like `.php` or `.aspx`,for example.

#### **Note**: Use the `-U` flag to list the parameters  for the given module and examples of usage
```shell
$  hydra http-post-form -U
```
#### In order to use the `http[s]-post-form` module we need to provide the following parameters separated by `:` 
1. **URL path**, which holds the login form
2. **POST parameters** for username/password
3. **A failed/success login string**, which lets hydra recognize whether the login attempt was successful or not

#### Example:
```hydra
/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:[FAIL/SUCCESS]=[success/failed string]
```

### Fail/Success String
#### For Hydra to recognize successfully submitted credentials and failed attempts, we need to specify a unique string from the source code of the page we're using to log in.
#### There are 2 different types of analysis:

* `Fail`    - Boolean Value: FALSE   - FLAG:  `F=html_content`
* `Success` - Boolean Value: TRUE - FLAG:  `S=html_content`

#### When we provide a fail string, it will keep searching until the string is **not found** in the response

#### The best option when looking for a string to use , is to pick something from the HTML source of the login page.
#### For instance, the login button.
```shell
"/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:F=<form name='login'"
```