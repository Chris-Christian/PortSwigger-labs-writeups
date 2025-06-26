## Solution steps

- Submit a random string in the search box, then inspect the web page using devloper tools.
- Find your Entered string with the help of `Ctrl+f`.
- Observe that the random string has been reflected inside a JavaScript string: `var searchTerms = 'xyz';`
- Replace your input with the following payload to break out of the JavaScript string and inject an alert: `xyz';alert(1)//`
- Now the script becomes:  `var searchTerms = 'xyz';alert(1)//';`
- `//` comments out the rest to avoid syntax errors.
- When the page loads, your alert should pop up.
