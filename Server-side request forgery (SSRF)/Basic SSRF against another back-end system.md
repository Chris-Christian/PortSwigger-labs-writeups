## Solution steps

- Open Burp suite and set-up proxy.
- Visit a product, click Check stock, intercept the request in Burp Suite, and send it to Burp Intruder.
- Change the `stockApi` parameter to `http://192.168.0.1:8080/admin` then highlight the final octet of the IP address (the number 1) and click Add `§`.
- In the Payloads side panel, change the payload type to Numbers, and enter 1, 255, and 1 in the From and To and Step boxes respectively.
- Start attack.
- Click on the Status column to sort it by status code ascending. You should see a single entry with a status of `200`, showing an admin interface.
- Click on this request, send it to Burp Repeater, and change the path in the stockApi to: `/admin/delete?username=carlos`
