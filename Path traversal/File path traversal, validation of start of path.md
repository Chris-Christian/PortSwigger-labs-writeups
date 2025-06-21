## Solution steps

- Open Burp Suite and intercept a request that fetches a product image.
- Keep the starting path as it is and modify the filename parameter, giving it the value: /var/www/images/../../../etc/passwd
- Send the request and observe that the response contains the contents of the /etc/passwd file.
