## Solution steps

- Enter a random string in the search box like: `hello`
- View page source and notice it gets reflected in a javascript
  <pre>`<script>
    var searchTerms = 'hello';
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
  </script>` </pre>
- 
