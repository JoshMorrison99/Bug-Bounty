## CSRF Methodology
<br/>

**Step 1**<br/>
The first thing to check in a CSRF attack is the security headers on the session cookie.
#### `SameSite=Strict` - CSRF Not Possible

```
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=strict;
```

#### `SameSite=LAX` - CSRF Only Possible on GET Request

```
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=lax;
```

#### `SameSite=NONE` - CSRF Possible

```
Set-Cookie: SessionId=CfDJ8MWK4x...; samesite=none;
```

