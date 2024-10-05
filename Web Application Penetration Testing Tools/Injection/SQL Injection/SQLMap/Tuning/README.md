# Attack Tuning
***
#### Each payload sent to the target consists of:
* **Vector** (e.g., `UNION ALL SELECT 1,2,VERSION()`) : The core part of the payload containing the SQL code intended to be executed on the target system
* **Boundaries** (e.g., `'<vector>-- -`): The prefix and suffix that properly embed the vector into the vulnerable SQL statement

### Prefix/Suffix
#### When we need special prefix and suffix values, we can use the `--prefix` and `--sufix` options.
#### For example:
```bash
$ sqlmap -u "www.example.com/?q=1337" --prefix="%'))" --suffix="-- -"
```
#### If the vulnerable code at target is.
```php
$query = "SELECT id,name,surname FROM users WHERE id LIKE (('" . $_GET["q"] . "')) LIMIT 0,1";
$result = mysqli_query($link, $query);
```
#### The vector `UNION ALL SELECT 1,2,VERSION()`, combined with the prefix `%'))` and the suffix `-- -`, will produce a valid SQL statement on the target like the following:
```sql
SELECT id,name,surname FROM users WHERE id LIKE (('test%')) UNION ALL SELECT 1,2,VERSION()-- -')) LIMIT 0,1
```
### Level/Risk
#### By default, SQLMap uses a predefined set of common boundaries (prefix/suffix pairs) and vectors that are most likely to succeed on a vulnerable target. However, users can opt to use larger sets of boundaries and vectors built into SQLMap.
#### To do this, the --level and --risk options can be applied:
* The --level option (ranging from 1 to 5, with 1 as the default) expands the range of vectors and boundaries based on their likelihood of success. The higher the level, the lower the expected success rate.
* The --risk option (ranging from 1 to 3, with 1 as the default) broadens the set of vectors based on the potential risk they pose to the target, such as causing data loss or denial-of-service.
#### To observe the differences in boundaries and payloads used with varying --level and --risk values, you can increase verbosity with the -v option. At verbosity level 3 or higher (e.g., -v 3), SQLMap will display messages showing the [PAYLOAD] used.
#### Payloads used with the default `--level` value have a considerably smaller set of boundaries

#### By default (`--level=1 --risk=1`), SQLMap tests up to 72 payloads for a single parameter. In the most thorough case (--level=5 --risk=3), this number increases to 7,865.
#### Since SQLMap is optimized to test the most common boundaries and vectors, regular users are generally advised not to adjust these settings, as doing so will significantly slow down the detection process. However, in specific cases, such as SQL injection vulnerabilities where OR payloads are necessary (e.g., login pages), you may need to increase the risk level.
#### This is because OR payloads can be risky in default scans, particularly when the vulnerable SQL statements might be modifying the database (e.g., through DELETE or UPDATE commands).

### Advanced Tuning
#### Status Codes
##### When dealing with large responses containing dynamic content, subtle differences between TRUE and FALSE responses can be detected through HTTP status codes. For instance, if a TRUE response returns a 200 code and a FALSE one returns 500, you can use the --code option to specify the TRUE response code (e.g., --code=200).

#### Titles
##### If the difference between responses is noticeable in the HTML page titles, you can use the `--titles` option to base the detection on the content of the <title> tag.

#### Strings
##### If a specific string (e.g., "success") appears in TRUE responses but not in FALSE ones, the `--string` option can be used to focus detection on that string alone (e.g., --string=success).

#### Text-only
##### To ignore hidden HTML content like <script>, <style>, or <meta> tags and focus only on visible text, use the --text-only option.

#### Techniques
##### If you need to restrict testing to specific SQLi techniques, you can use the `--technique` option. For example, to avoid time-based blind and stacking payloads while focusing on boolean-based blind, error-based, and UNION-query payloads, use --technique=BEU.

#### UNION SQLi Tuning
##### In some cases, UNION SQLi payloads need additional information to function. If you know the exact number of columns in the vulnerable query, you can specify this with `--union-cols` (e.g., `--union-cols=17). If the default values used by SQLMap (NULL and random integers) aren't compatible with the query's results, you can provide an alternative with --union-char='a'.
##### Additionally, if a UNION query requires an appendix like FROM <table> (common in databases like Oracle), you can specify it with --union-from (e.g., `--union-from=users`). The failure to automatically use the correct appendix might occur if the DBMS type isn't detected beforehand.




