## Solution steps

- Open Burp suite and set-up proxy.
- On Oracle databases, every `SELECT` statement must specify a table to select `FROM`. There is a built-in table on Oracle called `dual` which we can use for this purpose.
- Using Burp suite capture the request that sets the product category filter and send it to repeater.
- Determine the number of columns that are being returned by the query and which columns contain text data.
- Observe that the query is returning two columns using: `'UNION+SELECT+NULL,NULL+FROM+dual--`.
- Both of them contains text: `'UNION+SELECT+'a','a'+FROM+dual--`. Observe that the response is reflected in the webpage.
- Use the following payload to display the database version: `'+UNION+SELECT+BANNER,+NULL+FROM+v$version--`
