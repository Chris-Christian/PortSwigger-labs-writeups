## Solution steps

- Open Burp suite and set-up proxy.
- Visit the front page of the shop, and use Burp Suite to intercept and modify the request containing the `TrackingId` cookie.
- Modify the `TrackingId` cookie, changing it to: `TrackingId=x'||pg_sleep(10)--`
- This will concatenate `pg_sleep(10)` with the `TrackingId` in sql query.
- Submit the request and observe that the application takes 10 seconds to respond.
