## Solution steps

- Open Burp suite and set-up proxy.
- Log in using the admin credentials.
- Browse to the admin panel, promote `carlos` and confirm it by pressing `yes`.
- Send the confirmation HTTP request to Burp Repeater.
- Open a private/incognito browser window, and log in with the non-admin credentials.
- Copy the non-admin user's session cookie into the existing Repeater request, change the username to `wiener`, and send the request.
