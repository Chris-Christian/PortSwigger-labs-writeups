## Solution steps

- Open Burp Suite and set up proxy (Keep the intercept off).
- Log in to your account using the given credentials and notice the option for uploading an avatar image.
- Upload an arbitrary image, then return to your account page. Notice that a preview of your avatar is now displayed on the page.
- In the proxy history, notice that your image was fetched using a GET request to /files/avatars/. Send this request to Burp Repeater.
- On your system, create a file called exploit.php, containing a script for fetching the contents of Carlos's secret. For example: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
- Upload this script as your avatar. The response indicates that you are not allowed to upload files with a .php extension.
- In Burp's proxy history, find the POST /my-account/avatar request that was used to submit the file upload. In the response, notice that the headers reveal that you're talking to an Apache server. Send this request to Burp Repeater.
- In Burp Repeater, go to the tab for the POST /my-account/avatar request and find the part of the body that relates to your PHP file.
  1. Change filename to .htaccess
  2. Change Content-Type to text/plain
  3. Replace payload with AddType application/x-httpd-php .shell
- Send the request and observe that the file was successfully uploaded.
- Use the back arrow in Burp Repeater to return to the original request for uploading your PHP exploit.
- Change the value of the filename parameter from exploit.php to exploit.shell
- Send the request again and notice that the file was uploaded successfully.
- Switch to the other Repeater tab containing the GET /files/avatars/<YOUR-IMAGE> request. In the path, replace the name of your image file with exploit.shell and send the request. Observe that Carlos's secret was returned in the response.
- Submit the secret to solve the lab.
