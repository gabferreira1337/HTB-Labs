# Google Hacking
***

### Example 1 - Search for pages on a given website (example.org) containing the ***login** page.
```
intitle:login site:example.org
```
### Example 2 - Search for a type of document (pdf) with a keyword (food) from a target website (EXAMPLE).
```
EXAMPLE filetype:pdf food 
```

### More Advanced Google Operators
* cache:    - View cached version of a web page
  * Example: `cache:www.example.org`
* allinurl: - Search for pages containing all the query terms specified in the URL
  * Example: `allinurl: Example food`
* inurl:    - Query for pages in a given website where the URL has a certain word
  * Example: `inurl: www.example.org`
* allintitle:  - View only pages that contain certain words
  * Example: `allintitle: food 1337`
* allinanchor: - Search for pages containing all query terms specified in the anchor text on links to the page
  * Example: `allinanchor: best icecream in italy ` 
* inanchor:  - Search for pages containing the query term specified in the anchor text on links to the page
  * Example: `Anti-virus inanchor:McAfee`
* link:  - Query for websites or pages containing links to the specified website or page.
  * Example: - `link:www.example.org`
* related:  - View websites that are similar or related to the URL specified.
  * Example: `related:www.example.org`
* info:  - Query for information about a specified website.
  * Example: - `info:www.example.org `
* location:  - Query for websites or pages containing links to the specified website or page.
  * Example: - `location:www.example.org` 