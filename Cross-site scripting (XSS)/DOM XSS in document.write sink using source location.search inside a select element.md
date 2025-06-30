## Solution steps

- Go to any of the products and inspect the elements of the page.
- Search for javascript.
- Observe that the dangerous JavaScript extracts a storeId parameter from the location.search source. It then uses document.write to create a new option in the select element for the stock checker functionality.
- Now, Add a storeId query parameter to the URL and enter a random alphanumeric string as its value. Like this `/product?productId=1&storeId=random`.
- The URL can only have one `?`, which starts the query string. After that, every additional parameter must be separated with `&`, not another `?`.
- Request this modified URL and notice that your random string is now listed as one of the options in the drop-down list.
- Right-click and inspect the drop-down list to confirm that the value of your storeId parameter has been placed inside a select element.
- Change the URL to include a suitable XSS payload inside the storeId parameter: `/product?productId=1&storeId="></select><img src=x onerror=alert(1)>`
