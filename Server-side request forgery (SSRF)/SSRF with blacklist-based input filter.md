## Solution steps

- Open Burp suite and set-up proxy.
- Visit a product, click "Check stock", intercept the request in Burp Suite, and send it to Burp Repeater.
- Change the URL in the `stockApi` parameter to `http://127.0.0.1/admin` and observe that the request is blocked.
- Change the URL to `http://127.1/admin` and observe that the URL is blocked again.
- Obfuscate the "a" by double-URL encoding it to `%25%36%31`.
- Intercept the request again and change the URL to `http://127.1/%25%36%31dmin/delete?username=carlos`.
- Forward the request.
