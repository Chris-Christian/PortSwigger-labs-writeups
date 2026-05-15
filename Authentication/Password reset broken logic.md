## Solution steps

- Open Burp and set-up proxy.
- Navigate to the login page and click the `forgot password?` button.
- Enter your username: `wiener`.
- Click submit and open the **Email Client** to view the password reset email that was sent.
- Click the link in the email and reset your password to whatever you want.
- In Burp, go to **Proxy > HTTP history** and study the requests and responses for the password reset functionality.
- Observe that the reset token is provided as a URL query parameter in the reset email. Notice that when you submit your new password, the `POST /forgot-password?temp-forgot-password-token` request contains the username as hidden input. Send this request to Burp Repeater.
- In Burp Repeater, observe that the password reset functionality still works even if you delete the value of the `temp-forgot-password-token` parameter in both the URL and request body.
- This confirms that the token is not being checked when you submit the new password.
- In that same repeater tab remove the values of the `temp-forgot-password-token` in both URL and request body and change the username to `carlos` and send the request.
- The password of `carlos` has been changed to the new password you just set.
- Login as `carlos` with the new password to solve the lab.
