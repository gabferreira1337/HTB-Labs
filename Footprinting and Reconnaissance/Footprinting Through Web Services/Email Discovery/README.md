# Email Discovery
***
### [theHarvester]( )
#### By using this tool we can gather emails, subdomains, hosts, open ports, employee names and banners from different public sources such as search engines, PGP key servers, and the Shodan computer database.

#### Example:
```shell
$ theHarvester -d example.com -l 200 -b bing
```

#### The `-d` flag specifies the domain or name to search, `-l`specifies the number of results to be retrieved, and `-b` the data source. 