## Solution steps

- Enter any random string in the search box.
- View it's page source and observe the script where our string is reflected: ``var message = `0 search results for 'hello'`;``
- As you can see we are now dealing with template literals (backtick strings) in JavaScript.
- In JavaScript, template literals allow expression evaluation using `${...}` syntax.
- So, we will directly craft a payload which get's evaluated using template literals (backtick strings) and triggers an alert.
- Enter this payload to trigger an alert: `${alert(1)}`
