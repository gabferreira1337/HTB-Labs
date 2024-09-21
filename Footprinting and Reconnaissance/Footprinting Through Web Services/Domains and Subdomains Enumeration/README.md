# Domains and Subdomains Enumeration
***
#### By enumerating domains and subdomains from an organization we can gather useful information , such as organizational history, services and products, and contact information.


### [NetCraft](https://www.netcraft.com/tools) or [Pentest Tools](https://pentest-tools.com)
#### With Pentest tools or NetCraft  we can extract information about the organizational website, for example infrastructure, subdomain, background, network, etc., by inserting our target website's URL in the text field.

### [Sublist3r](https://github.com/aboul3la/Sublist3r)
```shell
$ sublist3r -h
```
#### Subdomain enumeration:
```shell
$ sublist3r -d example.com
```
### [Amass](https://github.com/owasp-amass/amass)
#### Normal Scan  (Passive + Active)
```shell
$ amass enum -d example.com -o normal_scan.txt
```
#### Active Scan
```shell
$ amass enum -active -d example.com -o active_scan.txt
```
#### Passive Scan
```shell
$ amass enum -passive -d example.com -o passive_scan.txt
```
