## Solution steps

- Open the lab and browse to `/.git` to reveal the lab's Git version control data.
- Download a copy of this entire directory using the command: `wget -r https://YOUR-LAB-ID.web-security-academy.net/.git/`
- Explore the downloaded directory using *Git cola*.
- Notice that there is a commit with the message `"Remove admin password from config"`.
- Look closer at the diff for the changed `admin.conf` file. Notice that the commit replaced the hard-coded admin password with an environment variable `ADMIN_PASSWORD` instead.
- Now the password will be read from a secure environment variable instead of being hardcoded in the source code.
- However, the hard-coded password is still clearly visible in the diff.
- Go to commit > undo last commit.
- Click on `admin.conf` and copy the hard-coded password.
- Go back to the lab and log in to the administrator account using the leaked password.
- To solve the lab, open the admin interface and delete `carlos`.
