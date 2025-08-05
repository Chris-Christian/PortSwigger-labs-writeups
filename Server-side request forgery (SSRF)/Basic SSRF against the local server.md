## Solution steps

- Open Burp suite and set-up proxy.
- Browse to `/admin` and observe that you can't directly access the admin page.
- Visit a product, click "Check stock", intercept the request in Burp Suite, and send it to Burp Repeater.
- Change the URL in the `stockApi` parameter to `http://localhost/admin`. This should display the administration interface.
- Observe the HTML to identify the URL to delete the target user: `http://localhost/admin/delete?username=carlos`
- Submit this URL in the `stockApi` parameter, to deliver the SSRF attack.
