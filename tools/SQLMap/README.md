# SQLMap
***
### [SQLMap](https://github.com/sqlmapproject/sqlmap) is an open-source tool written in Python that automates the process of detecting SQLi vulnerabilities
#### Simple Example
```bash
$ sqlmap -u "http://vulnerablesite/user.php?id=1337"
```

#### SQLMap is equipped with a robust detection engine and a wide array of features. It offers an extensive set of options and switches that allow for precise adjustment of various aspects, such as:
* Target connection
* Injection detection
* Fingerprinting
* Enumeration
* Optimization
* Protection detection and bypass using "tamper" scripts


#### Useful flags:
* -parse-error = parse the DBMS error (if any) and display them
* -t = output traffic to a file
* -v [level from 0-6] = verbose output
* --proxy = redirect traffic through a proxy
* --technique = specify the SQLi technique to be used (ex:`--technique=BEU`)
* --union-from = specify appendix at the end of a UNION query (ex:`--union-from=Dual`) 
##### Enumeration
* --dump = dump data
* --dump-all = dump all content from all databases
* --exclude-sysdbs = skip content from system databases
* -T = Specify table name
* -D = Specify database name
* --banner  = Database version banner 
* -C    = specify columns
* --current-user
* --current-db
* --tables
* --schema = retrieve structure of all the tables
* --where
* --search
* --passwords = retrieve database users password hashes
* --all --batch = Enumerate the target providing entire enumeration details
* --is-dba

#### Bypass Web Application Protections
* --csrf-token  = ex: --csrf-token="csrf_token"
* --randomize = ensure that each request has a unique value for a parameter ex: --randomize=id
* --eval = evaluate code before the request is  being sent to the target ex: --eval="import hashlib; h=hashlib.md5(id).hexdigest()"
* --proxy = ex: proxy="socks4://171.40.190.50:33333"
* --tor
* --check-tor
* --proxy-file = list of proxies to be used
* --skip-waf = produce less noise by skipping WAF identification
* --random-agent
* --tamper = modify requests before being sent to the target ex: --tamper=between,randomcase
* --list-tampers
* --chunked = Split Post requests body into chunks

#### OS Exploitation
* --file-read
* --file-write
* --file-dest

#### Supported injection types
##### This tool os the only that can successfully detect and exploit all known SQLi types
##### To see which SQLi types are supported by SQLMap , we can simpli input the following command:
```bash
sqlmap -hh
```
##### By inputting the above command we see that it returns **(default "BEUSTQ")**
* B: Boolean-based blind
* E: Error-based
* U: Union query-based
* S: Stacked queries
* T: Time-based blind
* Q: Inline queries

#### **NOTE**: The UNION query-based SQLi, is considered the fastest over all the other types, because it can retrieve a limited amount of data called "chunks" through each request

#### Out-of-Band SQLi using SQLMap
##### SQLMap conducts an Out-of-band SQLi by making the target server request non-existent subdomains (e.g. `hello.attacker.com`). Here, "foo" represents the SQL response the attacker seeks. SQLMap captures these failed DNS requests and extracts the "foo" portion to reconstruct the full SQL response.

### Example of a SQLi vulnerable PHP code:
```php
$link = mysqli_connect($host, $username, $password, $database, 3306);
$sql = "SELECT * FROM users WHERE id = " . $_GET["id"] . " LIMIT 0, 1";
$result = mysqli_query($link, $sql);
if (!$result)
    die("<b>SQL error:</b> ". mysqli_error($link) . "<br>\n");
```
#### By analyzing the code above we see that error reporting is enabled for the vulnerable SQLi query, so in case of any query execution problems it will return a database error in the web-server response.
### Running SQLMap to test 
```bash
$ sqlmap -u "http://www.example.com/vuln.php?id=1" --batch
```
* --batch  = never ask for user input, use the default behavior

### Log Messages Description
#### Most common messages found during a scan of SQLMap:
#### URL content is stable
* Log Message: `"target URL content is stable"`
#### This means that there are no big changes between responses in case of continuous identical requests. So with that it is easier to spot differences caused by the potential SQLi attempts.
#### SQLMap has mechanisms to automatically remove the potential "noise" that could come from potentially unstable targets.

### Parameter appears to be dynamic
* Log Message: `"GET parameter 'id' appears to be dynamic"`
#### A "dynamic" parameter means that any changes made to its value would result in a change in the response. When the output is "static" so does not change, it could be an indicator that the value of the tested parameter is not processed by the target in the current context.


### Parameter might be injectable
* Log Message:  `"heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')"`
#### DBMS errors can be strong indicators of potential SQL injection vulnerabilities. In this instance, when SQLMap sent a deliberately malformed input (e.g., `?id=1",)..).))`), a MySQL error was triggered. This suggests that the tested parameter might be vulnerable to SQL injection and that the underlying database could be MySQL. However, it's important to note that this alone does not confirm the presence of SQL injection. It merely signals a possible vulnerability that requires further testing for verification.

### Parameter might be vulnerable to XSS attacks
* Log Message: `"heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks"
`
#### SQLMap also runs quick heuristic test for the presence of an XSS vulnerability.

### Back-end DMS is '...'
* Log Message: `"it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]"
`
#### SQLMap tests for all supported DBMSes in a normal run. Whe a specific DBMS is successfully detected we can filter the payloads to just that specific DBMS.

### Level/risk values
* Log Message: `"for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]"`

#### If it's evident that the target is using a specific DBMS, we can run targeted SQL injection tests specifically designed for that system. This involves executing all relevant payloads for that DBMS. However, if the DBMS isn't identified, only the most common payloads are tested.

### Reflective values found
* Log Message: `"reflective value(s) found and filtering out"`
#### Warning telling that parts of the payloads might appear in the response, potentially confusing automation tools. However, SQLMap can filter out this "junk" to ensure accurate comparisons with the original page content.

### Parameter appears to be injectable
* Log Message: `"GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="luther")"`
#### This message suggests that the parameter seems to be injectable, but there is still a possibility of a false positive. In cases like boolean-based blind or time-based blind SQLi, where false positives are common, SQLMap conducts thorough logic checks at the end of the run to eliminate them.
Additionally, when the --string="luther" option is used, SQLMap detects and relies on the consistent presence of the string "luther" in the response to differentiate between TRUE and FALSE outcomes. This is significant because it means advanced internal mechanisms like dynamicity removal or fuzzy comparison aren't needed, reducing the likelihood of false positives.

### Time-based comparison statistical model
* Log Message: `"time-based comparison requires a larger statistical model, please wait........... (done)"`
#### SQLMap uses a statistical model to identify  normal and intentionally delayed responses. To do this, it first gathers enough data on regular response times, allowing it to detect deliberate delays, even in high-latency networks

### Extending UNION query Injection technique tests
* Log Message: `"automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found"`
#### UNION-based SQL injection checks need significantly more requests to identify a working payload compared to other SQLi types. To reduce testing time, especially when the target seems non-injectable, SQLMap limits the number of requests for this check to 10. However, if another SQLi technique shows potential vulnerability, SQLMap increases the number of requests for UNION queries, anticipating a higher chance of success.

### Technique appears to be usable
* Log Message: `"ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test"`
#### Before testing UNION-based SQL injection, SQLMap uses a heuristic check with the ORDER BY technique to gauge its feasibility. If ORDER BY is usable, SQLMap can efficiently determine the correct number of UNION columns using a binary search method. This process, however, is dependent on the specific table involved in the vulnerable query.

### Parameter is vulnerable
* Log Message: `"GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]"`

#### This is a key message from SQLMap, indicating that a parameter is vulnerable to SQL injection. Typically, finding just one exploitable parameter may be sufficient for most users. However, if conducting a thorough assessment of the web application, it’s advisable to continue testing to identify and report all vulnerable parameters.

### SQLMap identified injection points
* Log Message: `"sqlmap identified the following injection point(s) with a total of 46 HTTP(s) requests:"`
#### Following after, is a list of all injection points, including the type, title, and payloads, serving as definitive proof of successfully detected and exploited SQLi vulnerabilities. SQLMap only lists findings that are confirmed to be exploitable and usable.


### Data logged to text files
* Log Message: `"fetched data logged to text files under '/home/user/.sqlmap/output/www.example.com'`
#### This specifies the local file system location where SQLMap stores all logs, sessions, and output data for a specific target, like www.example.com. Once an injection point is detected, all details for future runs are saved in session files within this directory. This allows SQLMap to minimize the number of requests needed for future tests by leveraging the data in these session files.

#### **NOTE** = We can use Curl syntax and simply switch the name with sqlmap.