## Objective

- The lab reflects user input into a JavaScript context, but certain characters are blocked (such as spaces and quotes).
- To solve the lab, we must:
  - Trigger a reflected XSS
  - Call the `alert()` function
  - Include the string "1337" in the alert message
  - Note: The payload only executes when you click "Back to blog" on the page.

## URL encoded payload:
```html
https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
```
### Step-by-Step Breakdown

1. Character Filtering Challenge
   - The application is blocking some characters, such as: Spaces (` `), Quotes (`"`) and Possibly `(`, `)`, and others.
   - We use tricks like:
     - `/**/` instead of spaces (JS comments work as whitespace)
     - Arrow functions (`=>`) instead of `function`
     - Comma operator (`,`) to include the value `1337`
2. Core Payload Function
   - `x = x => { throw/**/onerror=alert,1337 }`
   - We define a JavaScript arrow function called `x`.
   - Inside this function, we intentionally throw an error using `throw`.
   - We assign `onerror = alert`, which means: If any error happens, `alert()` should be called.
   - `,1337` is added to make sure `"1337"` appears in the alert or error stack.
3. Triggering the Payload Execution
   - We assign: `toString = x`
   - This means that when something tries to convert an object to a string, our custom function `x()` will run.
   - Then we trigger that using: `window + ''`
   - This forces `window` to be converted to a string.
   - Because we changed `window.toString` to our `x` function, it gets executed.
   - Inside that function, we throw an error.
   - And since `onerror = alert`, the alert pops up.
   - The alert contains "1337", satisfying the lab’s condition.
   - `{x:'` in the end is a harmless-looking object literal. It helps balance or complete the surrounding JavaScript so that our malicious payload doesn’t break execution.
4. Why it Works When Clicking “Back to blog”
   - The “Back to blog” link likely uses JavaScript (like javascript:...) to load or redirect.
   - When you click it, the reflected JavaScript payload is evaluated in the browser.
   - That’s when your crafted payload actually runs and triggers the alert.
