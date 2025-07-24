## Solution steps

- Open Burp suite and set-up proxy (Keep intercept off).
- Log in using the supplied credentials and access your account page.
- Use the provided feature to update the email address associated with your account.
- Go to Http history and click on the `POST` request `/myaccount/change-email`. Observe that the response contains your role ID.
- Send the request to repeater.
- Add `"roleid":2` into the JSON in the request body.
  ```html
  {
  "email":"test@email.com",  "roleid":2
  }
- Send the request.
- Observe that the response shows your `roleid` has changed to 2.
- Browse to `/admin` and delete `carlos`.
