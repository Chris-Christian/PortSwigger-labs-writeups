## Solution steps

- Open Burp suite and set-up proxy (Keep intercept off).
- Log in using the supplied credentials and access the user account page.
- Change the "id" parameter in the URL to `administrator`.
- View the response in Burp and observe that it contains the administrator's password.
- Log in to the administrator account and delete `carlos`.
