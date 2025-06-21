## Solution steps

- Open Burp suite.
- Modify a request that fetches a product image as this lab contains a path traversal vulnerability in the display of product images.
- Modify the filename parameter, giving it the value: ../../../etc/passwd
- Observe that the response contains the contents of the /etc/passwd file.
