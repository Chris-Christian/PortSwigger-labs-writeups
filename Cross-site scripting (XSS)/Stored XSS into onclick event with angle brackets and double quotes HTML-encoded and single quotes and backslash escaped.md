## Solution steps

- Open Burp Suite and set-up proxy (keep the intercept off).
- Open any blog and write down a random comment with random name, email and website.
- From Http history, review `GET /post?postId=8` and notice our website is reflected in into an `onclick()` event: `<a id="author" href="http://chris.com" onclick="var tracker={track(){}};tracker.track('http://chris.com');">`
- Let's try to breakout and trigger an alert.
- Try emtering the following payload as website: `http://chris.com');alert(1);//`
- Post the comment and refresh the page. Click on the name and notice that no alert is triggered.
- Possible reason: The payload uses raw characters `(', ;, etc.)`, but the application likely encodes or escapes special characters before embedding it in the page. So `'` becomes `&#39;` or `&apos;`, and that breaks the injection attempt.
- Enter this payload as website: `http://chris?&apos;-alert(1)-&apos;`
- Post the comment and click on the name and notice you have successfully triggered an alert.

### Why it worked?
- `&apos;` → becomes `'` in the browser (apostrophe/single quote)
- So final JS becomes: `tracker.track('http://chris?'-alert(1)-'');`
- JavaScript tries to evaluate the expression using subtraction `(-)`, so it must first execute `alert(1)`. Even though the subtraction itself results in `NaN`, the `alert(1)` runs as part of the expression evaluation.
- The last `&apos;` closes the outer string again, avoiding any syntax error.
