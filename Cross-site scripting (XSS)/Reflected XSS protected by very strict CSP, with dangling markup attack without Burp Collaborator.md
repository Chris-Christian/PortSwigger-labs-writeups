## Solution steps

- This lab uses:
  - A very strict Content Security Policy (CSP).
  - A dangling markup attack, which is the key here.
- What is a "dangling markup" attack?
  - It's when you inject incomplete HTML tags or attributes to "break out" of the existing structure and trick the browser into interpreting your input as part of a new element or event handler.
- Login using the provided credentials.
- Edit the url to test the email field: `/my-account?id=wiener&email=chris`
- Notice that we can successfully inject in the field from the url.
- Inspect the page using `Ctrl+shift+c`.
- Notice there's csrf token just after the email `<input required="" type="hidden" name="csrf" value="ZjdeRuvJJb2HfWUrkwCNXnuJZ2PEYxAL">`
- We can also escape the email input element by adding `">`: `/my-account?id=wiener&email=">chris`
- 
