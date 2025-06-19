## Solution steps

- Use Burp Suite to intercept and modify the request that sets the product category filter.
- Determine the number of columns that are being returned by the query by increasing the number of NULL values in the following query: '+UNION+SELECT+NULL--
- Verify that the query is returning three columns, using the following payload in the category parameter: '+UNION+SELECT+NULL,NULL,NULL--
- Try replacing each null with random values: '+UNION+SELECT+'a',NULL,NULL--
- If an error occurs, move on to the next NULL and try that instead.
- Observe that the second column is compatible with string data.
- Replace the random value with the value provided by the lab to solve it.
