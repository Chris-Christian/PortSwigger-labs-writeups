## Solution steps

- Open Burp Suite and set-up proxy.
- In the website search for a random string using search bar, such as `hello`
- Go to Http-history tab and view contents of `GET /search-results?search=hello`
- Notice that the string is reflected in a JSON response called search-results: `Content-Type: application/json;`
- Go to the "Target" tab in Burp. Click on the "Site map" sub-tab.
- From the Site Map, open the searchResults.js file and notice that the JSON response is used with an eval() function call: `eval('var searchResultsObj = ' + this.responseText);`
- `eval()` takes the raw JSON response from /search-results?... and executes it as JavaScript code. This is dangerous because if you can control the contents of `this.responseText`, you can inject your own JavaScript code.
- Test with `"hello` → see that it becomes `\"`  → confirms quotes are escaped: `"searchTerm":"\"hello"`
- Test with `\hello` → see that it's not escaped: `"searchTerm":"\hello"`
- Use `\"hello` to break out of string: `"searchTerm":"\\"hello"`
- Enter this payload to solve the lab: `\"-alert(1)}//`
  
### Explanation

- `\"` breaks out of the current string safely.
- `-alert(1)` is a valid JavaScript expression and avoids syntax errors.
- `}` closes the object.
- `//` comments out the rest of the code to prevent crashes.
- Final request: `GET /search-results?search=\"-alert(1)}//`
- Once injected, the eval() function runs: `var searchResultsObj = {"searchTerm":"\"-alert(1)}//", "results":[]};`
- This triggers `alert(1)`, confirming JavaScript execution.
