# Running SQLMap on an HTTP Request 
***
### Utilizing "copy as cURL" feature from within the Network Monitor panel inside the Dev Tools
#### To use SQLMap we just need to switch with curl
#### Example:
```bash
$ sqlmap 'http://www.1337.co/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```
#### To test for SQL injection with SQLMap, you need to provide either a parameter value that can be checked for vulnerabilities or use specific options like --crawl, --forms, or -g for automatic detection of parameters.

### GET/POST Requests
#### **GET** parameters are provided by using the flag `-u` / `--url`
#### For **POST** data, we can use the `--data` flag, as follows:
```bash
$ sqlmap 'http://www.1337.co/' --data 'uid=1&name=admin'
```
#### If we know that a certain parameter is prone to an SQLi vulnerability, we could narrow down the tests to only that parameter using `-p uid` . Or by marking inside the provided data with the `*`.
#### For example:
```bash
$ sqlmap 'http://www.1337.co/' --data 'uid=1*&name=admin'
```

### Full HTTP requests
#### To specify a complex HTTP request we can use the -r flag to read the given request from a file
#### Example:
```bash
$ sqlmap -r req1.txt
```
#### **Note**:We can specify the parameter that we want to inject in with an `*`, for example: `/?id=*`.

### Custom SQLMap Requests
#### Specifying the (session) cookie value using the `--cookie` option:
```bash
$ sqlmap ... --cookie='PHPSESSID=ab4561f4a6d10458458fa8b0eadac29d'
```
#### Using the `H/--header` for the same purpose:
```bash
$ sqlmap ... -H='Cookie:PHPSESSID=ab4561f4a6d10458458fa8b0eadac29d'
```
#### Another useful options to specify the same HTTP header's values are: `--host`, `--referer`, and `-A/--user-agent`, which are used to specify the same HTTP headers' values.
#### We can use the `--random-agent` switch that's deigned to randomly select a **User-agent** header value. This switch is an important thing to remember because certain protection solutions may automatically drop all HTTP traffic containing the default SQLMap's User-agent value (e.g. **User-agent: User-agent: sqlmap/1.8.0.0#dev (http://sqlmap.org)). Also we can use the `--mobile` option to imitate the smartphone by using that same header value.
#### With SQLMap is possible to test the headers for the SQLi vulnerability as well. We can achieve that by specifying the "custom" injection mark after the header's value (e.g. `--cookie="id=1*"`)
#### Lastly, to be able to specify an alternative HTTP method, we can use the `-method` option , as follows:
```bash
$ sqlmap -u www.target.com --data='id=7' --method PUT
```

### Custom HTTP Requests
#### SQLMap also supports JSON formatted and XML formatted HTTP requests
#### IN order to be able to use those formats we can use the `--cookie` option or simply using the `-r` flag to read from a file
