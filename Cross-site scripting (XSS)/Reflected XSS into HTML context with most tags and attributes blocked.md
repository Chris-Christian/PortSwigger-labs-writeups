## Solution steps

- Open Burp Suite and set-up a proxy.
- Inject a standard XSS vector, such as: `<img src=1 onerror=print()>`
- Observe that this gets blocked.
- Open Burp's browser and use the search function in the lab. Send the resulting request to Burp Intruder.
- In Burp Intruder, replace the value of the search term with: <>
- Place the cursor between the angle brackets and click Add § to create a payload position. The value of the search term should now look like: <§§>
- Visit the XSS cheat sheet and click Copy tags to clipboard.
- In the Payloads side panel, under Payload configuration, click Paste to paste the list of tags into the payloads list. Start attack.
- When the attack is finished, review the results. Note that most payloads caused a `400` response, but the `body` payload caused a `200` response.
- Go back to Burp Intruder and replace your search term with: `<body%20=1>`
- Place the cursor before the `=` character and click Add `§` to create a payload position. The value of the search term should now look like: `<body%20§§=1>`
- Visit the XSS cheat sheet and click Copy events to clipboard.
- In the Payloads side panel, under Payload configuration, click Clear to remove the previous payloads. Then click Paste to paste the list of attributes into the payloads list. Click Start attack.
- When the attack is finished, review the results. Note that many paylaods caused `400` response while some of them caused `200` response including `onresize` payload.
- Go to the exploit server and paste the following code, replacing `YOUR-LAB-ID` with your lab ID: `<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>`
- Click Store and Deliver exploit to victim.

### Explanation

-  Inside the src URL (payload): `?search=%22%3E%3Cbody%20onresize=print()%3E`
-  When URL-decoded, it becomes: `?search="><body onresize=print()>`
-  So, the full iframe src becomes: `https://YOUR-LAB-ID.web-security-academy.net/?search="><body onresize=print()>`
-  `?search=">` → Closes any open attribute or string and breaks out into HTML context.
-  `<body onresize=print()>` → Injects a new `<body>` tag with an event handler.
-  `onresize=print()` → Calls `print()` when the body is resized (a browser built-in JS function that opens the print dialog).
-  Back to the iframe tag: `onload=this.style.width='100px'`
-  Once the iframe loads, this JS executes and changes the iframe's width to `100px`, likely to hide it visually.
