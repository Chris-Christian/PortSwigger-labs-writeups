## Solution steps

- Open any one blog post and view it's page source.
- Locate the following script tag: `<script src='/resources/js/loadCommentsWithVulnerableEscapeHtml.js'></script>`
- Navigate to the script URL `(/resources/js/loadCommentsWithVulnerableEscapeHtml.js)` and review the contents.
- Analyze the escapeHTML() function inside the script:
    <pre>   function escapeHTML(html) {
    return html.replace('&lt;', '&lt;').replace('&gt;', '&gt;'); 
    }</pre>
- This function only replaces the first occurrence of `<` and `>`, due to missing the global flag in the `.replace()` method.
- Additionally, it fails to escape other critical characters like `&`, `"`, `'`, and `/`, making it inadequate for secure HTML rendering.
- So craft a payload like this: `<><img src=1 onerror=alert(1)>`. Submit it as a comment with random name and email.
- Since only the first `<` and `>` are escaped, the rest of the HTML is parsed as actual DOM content.
- Revisit the blog post. The XSS should trigger, showing a JavaScript alert.
