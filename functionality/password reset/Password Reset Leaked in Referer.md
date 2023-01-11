If the password reset token is leaked in the `referer` header, it allows the person who has control of particular site to change the user’s password, because this person knows reset password token of the user.

************Step 1************

Request password reset to your email address

************Step 2************

Click on the password reset link

************Step 3************

Don’t change password

************Step 4************

Click any 3rd party websites(eg: Facebook, twitter)

************Step 5************

Intercept the request in burpsuite proxy

************Step 6************

Check if the `referer` header is leaking password reset token.
