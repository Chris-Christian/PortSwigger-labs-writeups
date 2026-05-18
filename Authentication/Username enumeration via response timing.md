## Solution steps

- Open Burp and set-up proxy.
- With Burp running, submit an invalid username and password, then send the `POST /login` request to Burp Repeater. Experiment with different usernames and passwords. Notice that your IP will be blocked if you make too many invalid login attempts.
- Add `X-Forwarded-For` header with value `1` to check if the header is supported. It allows you to spoof your IP address and bypass the IP-based brute-force protection.
- Continue experimenting with usernames and passwords. Pay particular attention to the response times. Notice that when the username is invalid, the response time is roughly the same. However, when you enter a valid username (your own), the response time is increased depending on the length of the password you entered.

- <img width="1365" height="176" alt="image" src="https://github.com/user-attachments/assets/58182638-a4f9-4eb8-8cf5-5785cecf38a9" />

- 
