## Solution steps

- Open Burp Suite and set up proxy (Keep the intercept off).
- Log in to your account using the given credentials and notice the option for uploading an avatar image.
- Upload an arbitrary image, then return to your account page. Notice that a preview of your avatar is now displayed on the page.
- In the proxy history, notice that your image was fetched using a GET request to /files/avatars/<YOUR-IMAGE>. Send this request to Burp Repeater.
- On your system, create a file called exploit.php, containing a script for fetching the contents of Carlos's secret file. For example: <?php echo file_get_contents('/home/carlos/secret'); ?>
- Use the avatar upload function to upload your malicious PHP file.
- The file is successfully uploaded.
- In Burp Repeater, change the path of the request to point to your PHP file: GET /files/avatars/exploit.php HTTP/1.1
- Send the request. Notice that the server has executed your script and returned its output (Carlos's secret) in the response.
- Submit the secret to solve the lab.
