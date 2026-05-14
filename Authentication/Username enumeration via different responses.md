## Solution steps

- Open Burp Suite and set-up proxy.
- Keep the intercept off.
- Navigate to the login page and submit an invalid username and password.
- In Burp, go to **Proxy > HTTP** history and find the `POST /login` request.
- Right click and send the request to intruder.
- Select the value of the `username` parameter in the request and click `Add §` button.
- Notice that the `username` parameter is set as a payload position: `username=§test§`
- Leave the password as any static value for now.
- Make sure that Sniper attack is selected.
- In the Payloads side panel, make sure that the Simple list payload type is selected.
- Under Payload configuration, paste the list of candidate usernames.
- Start the attack.
- When the attack is finished, examine the Length column in the results table. You can click on the column header to sort the results. Notice that one of the entries is longer than the others. Compare the response to this payload with the other responses.
- Notice that other responses contain the message `Invalid username`, but this response says `Incorrect password`. Make a note of the username in the **Payload** column.
- Close the attack and go back to the Intruder tab. Click `Clear §`, then change the `username` parameter to the username you just identified. Add a payload position to the `password` parameter. The result should look something like this: `username=identified-user&password=§random§`
- In the Payloads side panel, clear the list of usernames and replace it with the list of candidate passwords.
- Start the attack.
- When the attack is finished, look at the **Status** column. Notice that each request received a response with a `200` status code except for one, which got a `302` response. This suggests that the login attempt was successful. Make a note of the password in the **Payload** column.
- Log in using the username and password that you identified and access the user account page to solve the lab.

> **Note**: It's also possible to brute-force the login using a single cluster bomb attack. However, it's generally much more efficient to enumerate a valid username first if possible.
