## Solution steps

Background: AngularJS is a popular JavaScript framework that scans the contents of HTML nodes containing the ng-app attribute (also known as an AngularJS directive). When a directive is added to the HTML code, it allows execution of JavaScript expressions within double curly braces `{{ }}`. This technique becomes useful especially when angle brackets (`<`, `>`) are being encoded, making traditional XSS injection harder.

- Enter a random alphanumeric string into the search box.
- Inspect the elements of the page.
- Observe that your random string is enclosed in an ng-app directive: `<body ng-app="" class="ng-scope">`
- Enter the following AngularJS expression in the search box: `{{$on.constructor('alert(1)')()}}`
