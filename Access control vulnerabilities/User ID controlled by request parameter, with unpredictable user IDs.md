## Solution steps

- Find a blog post by `carlos`.
- Click on `carlos` and observe that the URL contains his user ID. Make a note of this ID.
- Open Burp suite and set-up proxy.
- Turn the intercept `ON`.
- Login using the given credentials.
- Forward the `POST` request containing your `username` and `password`.
- Change the user ID of `wiener` in the `GET` request with user ID of `carlos` and forward the request.
- Observe that you're now logged-in as carlos.
- Copy the API key and submit it.
