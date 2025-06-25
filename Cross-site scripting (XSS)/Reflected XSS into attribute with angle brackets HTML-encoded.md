## Solution steps

- Submit a random string in the search box and inspect the page's elements and search for your entered string.
- Observe the line: `<input type=text placeholder='Search the blog...' name=search value="RandomString">`
- Here `<script>alert(1)</script>` won't work because It becomes: `<input value="<script>alert(1)</script>">`
- That’s invalid HTML. The browser doesn’t treat it as a real <script> tag inside an attribute, it just gets escaped or ignored. You can’t execute `<script>` tags inside attribute values like that.
- Replace your random string with: `" onmouseover="alert(1)`
- When you hover over the search box your alert gets triggered because it becomes: `<input value="" onmouseover="alert(1)">`
