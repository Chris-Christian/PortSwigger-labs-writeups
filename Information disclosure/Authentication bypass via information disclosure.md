## Solution steps

- Open Burp suite and set-up proxy (Keep intercept off).
- Navigate to `/admin` and observe the response: `Admin interface only available to local users`
- From Http history, send that request to Burp repeater.
- From repeater, send the request again, but this time use the `TRACE` method: `TRACE /admin`
- Notice that the `X-Custom-IP-Authorization` header, containing your IP address, was automatically appended to your request. This is used to determine whether or not the request came from the `localhost` IP address.
- Go to Proxy > Match and replace.
- Under HTTP match and replace rules, click Add. The Add match/replace rule dialog opens.
- In the Replace field, enter the following: `X-Custom-IP-Authorization: 127.0.0.1`
- Click Test.
- Under Auto-modified request, notice that Burp has added the `X-Custom-IP-Authorization` header to the modified request.
- Browse to the home page. Notice that you now have access to the admin panel, where you can delete `carlos`.
