## Solution steps

- Open Burp suite and set-up proxy.
- Visit a product, intercept the request in Burp Suite, and send it to Burp Repeater.
- Modify the **Referer header** to point to a **Burp Collaborator payload**, like: `http://abcd1234.burpcollaborator.net`
- Send the request.
- In the Collaborator tab (Pro only), click **Poll now** to check for interactions.
- If the server made a request to your payload domain, you’ll see **HTTP/DNS logs**, confirming the SSRF.

### ⚠️ Note
This lab requires **Burp Suite Professional** for Burp Collaborator. As I’m using the **Community Edition**, I couldn’t test it live but studied the request flow and interaction mechanism.
