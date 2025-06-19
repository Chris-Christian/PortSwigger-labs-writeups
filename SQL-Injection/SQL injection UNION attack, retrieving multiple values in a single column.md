## Solution steps

- Use Burp Suite to intercept and modify the request that sets the product category filter.
- Determine the number of columns that are being returned by the query by increasing NULL values untill the error disappears using the following payload: '+UNION+SELECT+NULL--
- Observe that the error disappears after two NULL values. Hence, we can conclude that two columns are being returned by the query.
- Determine which columns contain text data: '+UNION+SELECT+'a',NULL--
- Only the second column contains text data.
- Use the following payload to retrieve the contents of the users table using only one column with '~' acting as a separator between username and password: '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
- Verify that the application's response contains usernames and passwords.
