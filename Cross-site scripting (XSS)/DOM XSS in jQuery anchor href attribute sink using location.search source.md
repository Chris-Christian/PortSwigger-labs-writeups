## Solution steps

- There's no search box. Go to the Submit feedback page.
- Observe it's url.
- Change the query parameter returnPath to / followed by a random string.
- Right-click and inspect the element, press Ctrl+f and search for the random string you entered in the previous step.
- Hover over the line related to your entered string.
- Observe that your random string appears inside an anchor `<a>` tag that functions as a JavaScript-powered back button.
- Change returnPath from the url to: `javascript:alert(document.cookie)`
- Hit enter and click "back".
