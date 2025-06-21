## Solution steps

- Open Burp Suite and intercept a request that fetches a product image.
- The application validates that the supplied filename ends with the expected file extension.
- ../../../etc/passwd.png won't work, so we append a null byte after passwd to strip the .png extension during execution: ../../../etc/passwd%00.png
