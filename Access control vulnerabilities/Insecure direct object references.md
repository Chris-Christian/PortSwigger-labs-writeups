## Solution steps

- Open Burp Suite and set-up proxy (Keep the intercept off).
- Select the Live chat tab.
- Send a message and then select View transcript.
- From Http history view `GET` request `/download-transcript/2.txt`.
- Send the request to repeater.
- Change the filename to `1.txt` and review the text. Notice a password within the chat transcript.
- Return to the main lab page and log in as `carlos` using the stolen credentials.
