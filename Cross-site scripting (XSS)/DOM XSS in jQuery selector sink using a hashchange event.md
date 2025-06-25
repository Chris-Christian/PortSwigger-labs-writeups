## Solution steps

- Notice the vulnerable code on the home page using the browser's DevTools.
- From the lab banner, open the exploit server.
- In the Body section, add the following malicious iframe: `<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>`
- Store the exploit, then click View exploit to confirm that the print() function is called.
- Go back to the exploit server and click Deliver to victim to solve the lab.

## Summary
The lab was vulnerable to DOM-based XSS due to insecure use of location.hash with jQuery’s $() selector. By injecting a malicious img tag with an onerror handler into the hash part of the URL, and delivering this through an iframe, I was able to execute arbitrary JavaScript (print()) in the victim's browser.
