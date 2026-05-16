## Solution steps

- Open Burp and set-up proxy.
- Try to login with any random username and password.
- From **Proxy > HTTP History** send the `POST /login` request to the intruder.
- Select the value of the `username` parameter and click `Add §`.
  - `username=§Tester§&password=random`
- In the **Payloads** side panel, make sure that the **Simple list** payload type is selected and add the list of candidate usernames.
- Click on the  **Settings** tab to open the **Settings** side panel. Under **Grep - Extract**, click **Add**. In the dialog that appears, scroll down through the response until you find the error message `Invalid username or password.`. Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted. Click **OK** and then start the attack.
- When the attack is finished, notice that there is an additional column containing the error message you extracted. Sort the results using this column to notice that one of them is subtly different.
- Look closer at this response and notice that it contains a typo in the error message - instead of a full stop/period, there is a trailing space. Make a note of this username.
- Close the results window and go back to the **Intruder** tab. Insert the username you just identified and add a payload position to the `password` parameter:
  - `username=identified-user&password=§random§`
- In the Payloads side panel, clear the list of usernames and replace it with the list of passwords. Start the attack.
- When the attack is finished, notice that one of the requests received a `302` Status code. Make a note of this password.
- Log in using the username and password that you identified and access the user account page to solve the lab.
