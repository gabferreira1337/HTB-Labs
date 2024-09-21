# Bypassing Web Application Protections
***
### Anti-CSRF token Bypass
#### SQLMap offers options to help bypass anti-CSRF (Cross-Site Request Forgery) protection mechanisms. One of the key options for this purpose is --csrf-token. By specifying the name of the token parameter (which should already be included in the request data), SQLMap will automatically parse the target's response to find the latest token values and include them in subsequent requests.
#### If the user does not manually specify the token name using --csrf-token, SQLMap will still attempt to identify it. If any of the provided parameters contain common infixes like "csrf," "xsrf," or "token," SQLMap will ask the user if they want to update the token in future requests.

```bash
$ sqlmap -u "http://www.example.com/" --data="id=1337&csrf-token=CdD4szMUHhioBx9AHFply3L2xOOfjRiJ" --csrf-token="csrf-token"
```

### Unique Value Bypass
```bash
$ sqlmap -u "http://www.example.com/?id=1337&rp=12345" --randomize=rp --batch -v 5 
```

### Calculated Parameter Bypass
```bash
$ sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI
```

### IP Address Concealing
#### To hide our IP address or bypass a web application's IP blocking mechanism, we can use a proxy or the **Tor** network. With SQLMap, you can configure a proxy using the `--proxy` option (e.g., `--proxy="socks4://177.39.187.70:33283"`), where you specify a valid proxy server.
### If we have a list of proxies, we can supply them to SQLMap using the `--proxy-file` option, allowing SQLMap to rotate through different proxies during its operations.


### WAF Bypass
####

### User-agent Blacklist Bypass
#### To bypass the blacklist we can set the switch `--random-agent`, to change the default user-gent with a random one

### Tamper Scripts
#### One of SQLMap's key techniques for bypassing Web Application Firewalls (WAF) or Intrusion Prevention Systems (IPS) is the use of "tamper" scripts. These are custom Python scripts designed to alter requests just before they are sent to the target, typically to evade security measures.
#### For example, a commonly used tamper script replaces the greater than (`>`) operator with `NOT BETWEEN 0 AND #` and the equals (`=`) operator with `BETWEEN # AND #`. This approach can bypass many basic security mechanisms, particularly those aimed at blocking XSS attacks, making it effective for SQL injection.
#### Multiple tamper scripts can be combined using the `--tamper` option (e.g., `--tamper=between,randomcase`), allowing for more sophisticated evasion strategies.
#### Most used tamper scripts:
### Tamper-Script - Description
* **0eunion** = Replaces instances of UNION with e0UNION
* **base64encode** = Base64-encodes all characters in a given payload
* **between** = Replaces greater than operator (>) with NOT BETWEEN 0 AND # and equals operator (=) with BETWEEN # AND #
* **commalesslimit** = Replaces (MySQL) instances like LIMIT M, N with LIMIT N OFFSET M counterpart
* **equaltolike** = Replaces all occurrences of operator equal (=) with LIKE counterpart
* **halfversionedmorekeywords** = Adds (MySQL) versioned comment before each keyword
* **modsecurityversioned** = Embraces complete query with (MySQL) versioned comment
* **modsecurityzeroversioned** = Embraces complete query with (MySQL) zero-versioned comment
* **percentage** = Adds a percentage sign (%) in front of each character (e.g. SELECT -> %S%E%L%E%C%T)
* **plus2concat** = Replaces plus operator (+) with (MsSQL) function CONCAT() counterpart
* **randomcase** = Replaces each keyword character with random case value (e.g. SELECT -> SEleCt)
* **space2comment**           = Replaces space character **( ) with comments `/
* **space2dash**      = Replaces space character **( ) with a dash comment (--) followed by a random string and a new line (\n)
* **space2hash** =  Replaces (MySQL) instance**s of space character ( ) with a pound character (#) followed by a random string and a new line (\n)
* **space2mssqlblank** = Replaces (MsSQL) instances of space character ( ) with a random blank character from a valid set of alternate characters
* **space2plus** = Replaces space character ( ) with plus (+)
* **space2randomblank**   = Replaces space character ( ) with a random blank character from a valid set of alternate characters
* **symboliclogical**          = Replaces AND and OR logical operators with their symbolic counterparts (&& and ||)
* **versionedkeywords** = Encloses each non-function keyword with (MySQL) versioned comment
* **versionedmorekeywords**  = Encloses each keyword with (MySQL) versioned comment
#### **Note:** To get a whole list of implemented tamper scripts, along with the description as above, switch --list-tampers can be used. We can also develop custom Tamper scripts for any custom type of attack, like a second-order SQLi.

### Miscellaneous Bypasses
#### We can use the `--chunks` flag , to split the POST request's body into "chunks". With this functionality we can bypass the SQL keywords blacklist.
#### Lastly we can use the HTTP parameter pollution bypass mechanism, (e.g. `?id=1337&id=UNION&id=SELECT&id=username,password&id=FROM&id=admin...),` )
