## Solution steps

- Log in to your own account. Your 2FA verification code will be sent to you by email. Click the Email client button to access your emails.
- Go to your account page and make a note of the URL.
  - `https://XXXX.web-security-academy.net/my-account?id=wiener`
- Log out of your account.
- Log in using the victim's credentials.
- When prompted for the verification code, manually change the URL to navigate to `https://XXXX.web-security-academy.net/my-account?id=carlos`.
- The lab is solved when the page loads.

> - The application checks the password, starts the 2FA process, but forgets to enforce completion of 2FA before granting access to protected pages.
