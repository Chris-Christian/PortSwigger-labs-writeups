## Solution steps

- Open Burp Suite and intercept a request that fetches a product image.
- Modify the filename parameter, giving it the value: ../../../etc/passwd
- URL encode it using Ctrl+u
- Forward it and observe that the response contains the contents of the /etc/passwd file.
