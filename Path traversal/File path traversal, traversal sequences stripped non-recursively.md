## Solution steps

- Open Burp Suite and intercept a request that fetches a product image.
- Modify the filename parameter, giving it the value: ../../../etc/passwd
- This doesn't work because the application strips path traversal sequences from the user-supplied filename before using it.
- We will modify the filename parameter, giving it the value: ....//....//....//etc/passwd
- Forward the request and observe that the response contains the contents of the /etc/passwd file.

It works because when the application strips the path traversal sequences from ....//....//....//etc/passwd we are left with ../../../etc/passwd
