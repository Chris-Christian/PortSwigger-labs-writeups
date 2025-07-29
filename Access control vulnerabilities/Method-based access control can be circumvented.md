## Solution steps

- Open Burp suite and set-up proxy (Keep intercept off).
- Log in using the admin credentials.
- Browse to the admin panel, promote `carlos`, and send the HTTP request to Burp Repeater.
- Open a private/incognito browser window, and log in with the non-admin credentials.
- Attempt to re-promote `carlos` with the non-admin user by copying that user's session cookie into the existing Burp Repeater request, and observe that the response says "Unauthorized".
- Change the method from `POST` to `POSTX` and observe that the response changes to "missing parameter".
- Convert the request to use the `GET` method by right-clicking and selecting "Change request method".
- Change the username parameter to your username (`wiener`) and resend the request.

### Note on Realism:

In a real-world attack, an attacker wouldn't have access to the admin request. However, this lab is designed to demonstrate insecure method-based access control, where the server authorizes based on the HTTP method rather than the resource or user role.
