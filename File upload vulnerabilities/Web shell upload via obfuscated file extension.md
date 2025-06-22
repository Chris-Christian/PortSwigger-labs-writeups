## Solution steps

- Open Burp Suite and set up proxy (Keep the intercept off).
- Log in to your account using the given credentials and notice the option for uploading an avatar image.
- Upload an arbitrary image, then return to your account page. Notice that a preview of your avatar is now displayed on the page.
- In the proxy history, notice that your image was fetched using a GET request to /files/avatars/. Send this request to Burp Repeater.
- On your system, create a file called exploit.php, containing a script for fetching the contents of Carlos's secret. For example: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
- Attempt to upload this script as your avatar. The response indicates that you are only allowed to upload JPG and PNG files.
- In Burp's proxy history, find the POST /my-account/avatar request that was used to submit the file upload. Send this to Burp Repeater.
- In Burp Repeater, go to the tab for the POST /my-account/avatar request and find the part of the body that relates to your PHP file. In the Content-Disposition header, change the value of the filename parameter to include a URL encoded null byte, followed by the .jpg extension: filename="exploit.php%00.jpg"
- Send the request and observe that the file was successfully uploaded. Notice that the message refers to the file as exploit.php, suggesting that the null byte and .jpg extension have been stripped.
- Switch to the other Repeater tab containing the GET /files/avatars/<YOUR-IMAGE> request. In the path, replace the name of your image file with exploit.php and send the request. Observe that Carlos's secret was returned in the response.
- Submit the secret to solve the lab.
