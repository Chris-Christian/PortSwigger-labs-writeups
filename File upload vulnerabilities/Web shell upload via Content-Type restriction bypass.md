## Solution steps

- Open Burp Suite and set up proxy (Keep the intercept off).
- Log in to your account using the given credentials and notice the option for uploading an avatar image.
- Upload an arbitrary image, then return to your account page. Notice that a preview of your avatar is now displayed on the page.
- In the proxy history, notice that your image was fetched using a GET request to /files/avatars/. Send this request to Burp Repeater.
- On your system, create a file called exploit.php, containing a script for fetching the contents of Carlos's secret.For example: `<?php echo file_get_contents('/home/carlos/secret');?>`
- Upload this script as your avatar.
- The response indicates that you are only allowed to upload files with the MIME type image/jpeg or image/png.
- In Burp, go back to the proxy history and find the POST /my-account/avatar request that was used to submit the file upload. Send this to Burp Repeater.
- In Burp Repeater, go to the tab containing the POST /my-account/avatar request. In the part of the message body related to your file, change the specified Content-Type to image/jpeg.
- Send the request. Observe that the response indicates that your file was successfully uploaded.
- Switch to the other Repeater tab containing the GET /files/avatars/<YOUR-IMAGE> request. In the path, replace the name of your image file with exploit.php and send the request.
- Observe that Carlos's secret was returned in the response.
- Submit the secret to solve the lab.
