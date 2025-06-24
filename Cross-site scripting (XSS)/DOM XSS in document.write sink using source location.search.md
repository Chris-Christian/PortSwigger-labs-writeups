## Solution steps

- Enter a random string into the search box.
- Right-click and inspect the element, and observe that your random string has been placed inside an img src attribute.
- Break out of the img attribute by searching for: `"><script>alert(1)</script>`  OR  `"><svg onload=alert(1)>`
