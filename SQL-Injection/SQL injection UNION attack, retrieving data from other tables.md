## Solution steps

- Use Burp Suite to intercept and modify the request that sets the product category filter.
- Determine the number of columns that are being returned by the query by increasing NULL values untill the error disappears using the following payload: '+UNION+SELECT+NULL--
- Observe that the error disappears after two NULL values. Hence, we can conclude that two columns are being returned by the query.
- Determine which columns contain text data: '+UNION+SELECT+'a','b'--
- Both the columns contain text data as no error occured.
- Use the following payload to retrieve the contents of the users table: '+UNION+SELECT+username,+password+FROM+users--
