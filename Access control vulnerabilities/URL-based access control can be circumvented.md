## Solution steps

- Open Burp suite and set-up proxy.
- Try to load `/admin` and observe that you get blocked. Notice that the response is very plain, suggesting it may originate from a front-end system.
- Turn on the intercept and refresh the page.
- Change the URL in the request line to `/` and add the HTTP header `X-Original-URL: /invalid`.
- Observe that the application returns a "not found" response. This indicates that the back-end system is processing the URL from the `X-Original-URL` header.
- Keep the intercept on and refresh the page again.
- Change the URL in the request line to `/` and the value of the `X-Original-URL` header to `/admin`.
- Observe that you can now access the admin page.
- Click on delete `carlos`.
- From the URL remove everything except `/?username=carlos` and change the `X-Original-URL` path to `/admin/delete`.
- The next intercepted request must be to access the admin page again after deleting the user. Change the URL in the request line to `/` and the value of the X-Original-URL header to `/admin`.
- Observe that the user `carlos` is deleted.
