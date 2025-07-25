## Solution steps

- Open Burp suite and set-up proxy.
- Log in using the supplied credentials and access your account page.
- Send the `GET` request to Burp Repeater.
- Change the "id" parameter to `carlos`.
- Observe that although the response is now redirecting you to the home page, it has a body containing the API key belonging to `carlos`.
- Submit the API key.
