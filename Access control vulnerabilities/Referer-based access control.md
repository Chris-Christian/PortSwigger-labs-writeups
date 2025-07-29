## Solution steps

- Open Burp suite and set-up proxy (Keep intercept off).
- Log in using the admin credentials.
- Browse to the admin panel, promote `carlos`, and send the HTTP request to Burp Repeater.
- Open a private/incognito browser window, and log in with the non-admin credentials.
- Browse to `/admin-roles?username=carlos&action=upgrade` and observe that the request is treated as unauthorized due to the absent Referer header.
- Copy the non-admin user's session cookie into the existing Burp Repeater request, change the username to `wiener`, and send it.
