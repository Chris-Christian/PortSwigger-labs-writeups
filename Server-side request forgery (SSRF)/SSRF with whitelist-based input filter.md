## Solution steps

- Open Burp suite and set-up proxy (Keep the intercept off).
- Visit a product, click "Check stock", intercept the request in Burp Suite, and send it to Burp Repeater.
- Change the URL in the `stockApi` parameter to `http://127.0.0.1/` and observe that the application is parsing the URL, extracting the hostname, and validating it against a whitelist.
- Change the URL to `http://username@stock.weliketoshop.net/` and observe that this is accepted, indicating that the URL parser supports embedded credentials.
- Append a `#` to the username and observe that the URL is now rejected.
- Double-URL encode the `#` to `%25%32%33` and observe the extremely suspicious "Internal Server Error" response, indicating that the server may have attempted to connect to "username".
- Change the `username` to `localhost:80`.
- To access the admin interface and delete the target user, change the URL to: `http://localhost:80%25%32%33@stock.weliketoshop.net/admin/delete?username=carlos`
