## Solution steps

- Use Burp Suite to intercept the request that sets the product category filter.
- Modify the category parameter, giving it the value '+UNION+SELECT+NULL--.
- Observe that an error occurs.
- Modify the category parameter to add an additional column containing a null value: '+UNION+SELECT+NULL,NULL--
- Continue adding null values until the error disappears.
- Observe that the error disappears after adding 3 null values and the response includes additional content containing the null values.
