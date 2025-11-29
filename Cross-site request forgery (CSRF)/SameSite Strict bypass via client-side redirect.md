## Solution steps

- Open Burp Suite and set-up proxy.
- Log in to your own account and change your email address.
- In Burp, go to the Proxy > HTTP history tab.
- Study the `POST /my-account/change-email` request and notice that this doesn't contain any unpredictable tokens, so may be vulnerable to CSRF if you can bypass any SameSite cookie restrictions.
- Look at the response to your `POST /login` request. Notice that the website explicitly specifies `SameSite=Strict` when setting session cookies.
- This prevents the browser from including these cookies in cross-site requests.
- In the browser, go to one of the blog posts and post an arbitrary comment. Observe that you're initially sent to a confirmation page at `/post/comment/confirmation?postId=x` but, after a few seconds, you're taken back to the blog post.
- In Burp, go to the proxy history and notice that this redirect is handled client-side using the imported JavaScript file /resources/js/commentConfirmationRedirect.js:
  ```
  redirectOnConfirmation = (blogPath) => {
      setTimeout(() => {
          const url = new URL(window.location);
          const postId = url.searchParams.get("postId");
          window.location = blogPath + '/' + postId;
      }, 3000);
  }
  ```
- Study the JavaScript and notice that this uses the `postId` query parameter to dynamically construct the path for the client-side redirect.
- In the proxy history, right-click on the `GET /post/comment/confirmation?postId=x` request and select Copy URL.
- In the browser, visit this URL, but change the postId parameter to an arbitrary string.
  `/post/comment/confirmation?postId=test`
- Observe that you initially see the post confirmation page before the client-side JavaScript attempts to redirect you to a path containing your injected string, for example, `/post/test`.
- Try injecting a path traversal sequence so that the dynamically constructed redirect URL will point to your account page:
  `/post/comment/confirmation?postId=1/../../my-account`
- Observe that the browser normalizes this URL and successfully takes you to your account page. This confirms that you can use the `postId` parameter to elicit a `GET` request for an arbitrary endpoint on the target site.
- In the browser, go to the exploit server and create a script that induces the viewer's browser to send the GET request you just tested. The following is one possible approach:
  ```
  <script>
      document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=../my-account";
  </script>
  ```
- Store and view the exploit yourself.
- Observe that when the client-side redirect takes place, you still end up on your logged-in account page. This confirms that the browser included your authenticated session cookie in the second request, even though the initial comment-submission request was initiated from an arbitrary external site.
- Send the POST `/my-account/change-email` request to Burp Repeater.
- In Burp Repeater, right-click on the request and select **Change request method**. Burp automatically generates an equivalent `GET` request.
- Send the request. Observe that the endpoint allows you to change your email address using a `GET` request.
- Go back to the exploit server and change the `postId` parameter in your exploit so that the redirect causes the browser to send the equivalent `GET` request for changing your email address:
  ```
  <script>
      document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
  </script>
  ```
- Note that you need to include the submit parameter and URL encode the ampersand delimiter to avoid breaking out of the postId parameter in the initial setup request.
- Change the email address in your exploit so that it doesn't match your own.
- Deliver the exploit to the victim. After a few seconds, the lab is solved.
