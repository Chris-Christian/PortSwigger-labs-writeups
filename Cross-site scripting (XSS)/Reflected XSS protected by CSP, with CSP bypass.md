## Solution steps

- Open Burp suite and set-up proxy (Keep the intercept off).
- Enter the following into the search box: `<img src=1 onerror=alert(1)>`
- Observe that the payload is reflected, but the CSP prevents the script from executing.
- In Http history, observe that the response contains a `Content-Security-Policy` header, and the `report-uri` directive contains a parameter called `token`. Because you can control the `token` parameter, you can inject your own CSP directives into the policy.
- This blocks any inline scripts (like `<script>alert(1)</script>`) unless `'unsafe-inline'` is explicitly allowed.
- Enter this payload in the url: `/?search=<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'`
- script-src vs. script-src-elem:
  - `script-src`: Controls all scripts (including inline and external).
  - `script-src-elem`: Controls only `<script>` tags inserted in the DOM, not event handlers or `eval()`.
- So if you provide a `script-src-elem` directive, it overrides the default `script-src` policy for `<script>` elements only.
- `;script-src-elem 'unsafe-inline'`: This injects a new CSP directive into the CSP header by terminating (`;`) the original policy and injecting a new one that allows inline scripts from `<script>` elements only.
