## Solution steps

- Enter any random string in the search box.
- View the page source and find the script where it is reflected:  var searchTerms = 'random';
- Ok so, to escape it let's try this payload: `';alert(1)//`
- It didn't trigger an alert. Let's review the source code:  `var searchTerms = '\';alert(1)//';`
- Here, the single quote `(')` is escaped by the backslash `(\)`, so it doesn't actually terminate the string. The entire payload becomes part of the string itself, rather than breaking out of it. As a result, the `alert(1)` is treated as plain text, not executable JavaScript.
- Now let's try adding another backslash before single quote: `\';alert(1)//`
- It successfully triggered the alert.
- Explanation: The double backslash `(\\)` in the source results in a single literal backslash `(\)` in the final string. Then the `\'` is interpreted as an escaped single quote within the string, which ends the string cleanly. After that, the injected `;alert(1)//` becomes actual JavaScript code, and the `//` comments out any remaining characters
