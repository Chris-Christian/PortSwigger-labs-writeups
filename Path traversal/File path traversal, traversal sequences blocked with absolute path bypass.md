## Solution steps

- Intercept a request using Burp Suite that fetches a product image.
- Modify the filename parameter, giving it the value /etc/passwd because the application blocks traversal sequences but treats the supplied filename as being relative to a default working directory..
- Observe that the response contains the contents of the /etc/passwd file.
