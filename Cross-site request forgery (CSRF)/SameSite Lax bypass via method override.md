## Solution steps

- Open Burp Suite and set-up proxy.
- Login using the provided credentials.
- Submit the "Update email" form, and find the resulting request in your Proxy history.
- Notice that this doesn't contain any unpredictable tokens, so may be vulnerable to CSRF if you can bypass the SameSite cookie restrictions.
- Look at the response to your `POST /login` request. Notice that the website doesn't explicitly specify any SameSite restrictions when setting session cookies. As a result, the browser will use the default `Lax` restriction level.
- This means cookies are blocked in most cross-site requests. But they ARE allowed for cross-site GET requests that cause a top-level navigation(when the main page in your browser changes to a new URL).
- Send the `POST /my-account/change-email` request to Burp Repeater.
- In Burp Repeater, right-click on the request and select Change request method. Burp automatically generates an equivalent `GET` request.
- Send the request. Observe that the endpoint only allows `POST` requests.
- Many web frameworks support _method to override the HTTP method.
- Try overriding the method by adding the _method parameter to the query string: `GET /my-account/change-email?email=test%40web-security-academy.net&_method=POST HTTP/1.1`
- Send the request. Observe that this seems to have been accepted by the server.
- Since the attack must be a top-level navigation `GET` request, let's use JavaScript to redirect the victim’s browser:
  ```
  <script>
      document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=pentester@web-security-academy.net&_method=POST";
  </script>
  ```
- When a victim visits this malicious page:
  1. Their browser navigates to the attacker-crafted URL.
  2. Because this is a top-level `GET` navigation, `SameSite=Lax` allows their session cookie to be sent.
  3. Server honors `_method=POST` and changes their email.
  4. CSRF is successful.
- Copy and paste the above script into the exploit server's body.
- Store the exploit, then click "Deliver to victim" to solve the lab.
