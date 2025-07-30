## Solution steps

- Open the lab's homepage and view the page source
- Scroll through the HTML and observe the presence of a commented-out debug link: `<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->`
- This reveals that the server hosts a publicly accessible `phpinfo.php` file, typically used for debugging.
- Visit the path: `https://<lab-url>/cgi-bin/phpinfo.php`
- Once the page loads, use `Ctrl+F` (Find) to search for the keyword: `SECRET_KEY`
- Copy the value shown next to `SECRET_KEY`.
- Return to the lab's main page and submit that value as the solution.
