## Solution steps

- Use Burp Suite to intercept the request that sets the product category filter.
- Determine the number of columns that are being returned by the query and which columns contain text data by nodifying the category parameter with the help of: '+UNION+SELECT+NULL,NULL#   and   '+UNION+SELECT+'abc','def'#
- Use the following payload to display the database version: '+UNION+SELECT+@@version,+NULL#
