In this scenario the password reset link doesn't expire after the user changes their email address. This can lead to an account takeover if someone where to register an account with the attacker's old email.

**Step 1**

In this scenario we are need two accounts. First, create `account1`

**Step 2**

Click Forgot Password and send the password reset link to `account1’s` email

**Step 3**

Go to your email client and don't open the password link, just copy it and paste into any editor.

**Step 4**

Click on Change Email in `account1’s` account and change the email to another email.

************Step 5************

Create `account2` using the email address of `account1’s` previous email

**Step 6**

Go to your password reset link which you copied and change your password (If the link still works).

**Step 7**

Logout of `account2` and try to log back in with `account2's` credentials. If the credentials were changed then this is vulnerable.

**Reference**

[https://hackerone.com/reports/685007](https://hackerone.com/reports/685007)
