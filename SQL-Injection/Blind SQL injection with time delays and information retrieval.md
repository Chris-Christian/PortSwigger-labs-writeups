## Solution steps

- Use Burp Suite to intercept and modify the request.
- Visit the front page of the shop, intercept the request containing the TrackingId cookie.
- Modify the TrackingId cookie, changing it to: TrackingId=x'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
- Verify that the application takes 10 seconds to respond.
- Now change it to: TrackingId=x'%3BSELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
- Verify that the application responds immediately with no time delay.
- Now change it to: TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
- Verify that the condition is true.
- The next step is to determine how many characters are in the password.
- To do this, change the value to: TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
- This condition should be true, confirming that the password is greater than 1 character in length.
- Using Burp repeater check the numbers sequentially until the condition stops being true (i.e. when the application responds immediately without a time delay).
- The condtion stops being true at password>20. Therefore, we can conclude that password length is 20 characters.
- Send the request to intruder.
- Change the value of the cookie to: TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
- This uses the SUBSTRING() function to extract a single character from the password, and test it against a specific value. Our attack will cycle through each position and possible value, testing each one in turn.
- Place payload position markers around the a character in the cookie value.
- In the Payloads side panel, check that Simple list is selected, and under Payload configuration add the payloads in the range a - z and 0 - 9. You can select these easily using the Add from list drop-down.
- To be able to tell when the correct character was submitted, you'll need to monitor the time taken for the application to respond to each request.
- For this process to be as reliable as possible, you need to configure the Intruder attack to issue requests in a single thread. To do this, click the Resource pool tab to open the Resource pool side panel and add the attack to a resource pool with the Maximum concurrent requests set to 1.
- Start the attack.
- Review the attack results.
- You should see a column in the results called Response received. This will generally contain a small number, representing the number of milliseconds the application took to respond. One of the rows should have a larger number in this column, in the region of 10,000 milliseconds. The payload showing for that row is the value of the character at the first position.
- Re-run the attack for each of the other character positions in the password, to determine their value.
- Continue this process testing offset 2, 3, and so on, until you have the whole password.
- Log in as the administrator user.

Note : While brute-forcing the password, you can use Cluster bomb attack as well, but it will take more time.
