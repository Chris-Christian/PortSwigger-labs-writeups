## Solution steps

- Open Burp suite.
- Observe that the stock check feature sends the productId and storeId to the application in XML format.
- Send the POST /product/stock request to Burp Repeater.
- In Burp Repeater, probe the storeId to see whether your input is evaluated.
- Try replacing the ID with mathematical expressions that evaluate to other potential IDs, for example: <storeId>1+1</storeId>
- Observe that your input appears to be evaluated by the application, returning the stock for different stores.
- Try determining the number of columns returned by the original query by appending a UNION SELECT statement to the original store ID: <storeId>1 UNION SELECT NULL</storeId>
- Observe that your request has been blocked due to being flagged as a potential attack.
- You need to bypass the WAF(web application firewall). A web application firewall (WAF) will block requests that contain obvious signs of a SQL injection attack.
- Download hackvertor from extensions->BAppStore.
- Try obfuscating your payload using XML entities using Hackvertor extension. Just highlight your input, right-click, then select Extensions > Hackvertor > Encode > dec_entities/hex_entities.
- Resend the request and notice that you now receive a normal response from the application. This suggests that you have successfully bypassed the WAF.
- When you try to return more than one column, the application returns 0 units, implying an error.
- As you can only return one column, you need to concatenate the returned usernames and passwords, for example: <storeId><@hex_entities>1 UNION SELECT username || ':' || password FROM users</@hex_entities></storeId>
- Send this query and observe that you've successfully fetched the usernames and passwords from the database, separated by a : character.
- Log in using administrator's credentials.
