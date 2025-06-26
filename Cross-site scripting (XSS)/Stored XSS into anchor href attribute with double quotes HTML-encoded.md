## Solution steps

- Open Burp Suite and set-up the proxy.
- Keeping the intercept off go to any of the posts and write a comment with random string and random name, email and website.
- After submitting, revisit the post and observe that your name appears as a hyperlink pointing to the website URL you entered.
- From Burp's HTTP history send the `POST post/comment` to Burp Repeater.
- In repeater, Change the website field to `javascript:alert(1)` and send the request.
- Go back to browser and refresh the page.
- Click the hyperlink on the name. It will trigger a JavaScript alert.
