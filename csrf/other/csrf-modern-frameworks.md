
**Summary**
Framework | Token Name | Vulnerability
---|---|---
Express | Custom | Developer implements state changing action in a GET request or developer forgets to check for CSRF token on endpoint or developer doesn't implement CSRF protection at all.
ASP.NET MVC | `__RequestVerificationToken` | Developer implements state changing action in a GET request or developer forgets to check for CSRF token on endpoint.
ASP.NET Razor Pages | `__RequestVerificationToken` | Developer implements state changing action in a GET request.
Ruby on Rails | `authenticity_token` | Developer implements state changing action in a GET request.


**Why would a web developer give the session cookie a value of SameSite: none?**
A web developer might give the session cookie a value of SameSite: none for scenarios where the website needs to use third-party cookies or perform cross-site requests. The SameSite attribute on a cookie defines the cookie's cross-site access, indicating whether the cookie should be sent along with requests initiated from third-party websites.

By setting the SameSite attribute to "none," the cookie can be accessed by other websites, allowing for a seamless user experience, such as when an embedded payment gateway needs to be accessed from a different domain. If the SameSite attribute is not set to "none," then cross-site requests will fail, which can cause issues for the user and potentially break functionality on the website.



## Express
Handling reqests in Express.js it is quite common to use JSON like so:
```js
app.post('/api/reset_password', async (req, res) => {
    console.log(req.body.password)
})
```

Sending the following request will reset the password:
```
POST /api/reset_password HTTP/1.1
Host: localhost:5000
Content-Type: application/json
Cookie: SESS_COOKIE=s%3AqpqwMG8nJEa6lR7sUkdS%2B2%2B%2FLnfYkCCA

{"password":"123"}
```

Notice in the request above that the Content-Type is `application/json`. CSRF is not possible in this Content-Type. Before attempting CSRF, check if any of the following Content-Types are accepted:
```
Content-Type: text/plain  
Content-Type: application/x-www-form-urlencoded  
Content-Type: application/xml
```

If an express application only allows JSON, then the middleware will look like so:
```js
app.use(express.json());
```

If the express application accepts `application/json` and `application/x-www-form-urlencoded `
```js
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

---

To implement CSRF Protection in express, the common practise is to use the following npm package:
```
https://www.npmjs.com/package/csurf
```

To implement `csurf` on the express app we add the following code:
```js
var csrf = require('csurf')

var csrfProtection = csrf({ cookie: true })
app.use(csrfProtection);

app.get('/api/csrf-token', (req, res) => {
    res.json({ CSRFToken: req.csrfToken() });
});
```

The code above is all we need to implement CSRF protection on the backend.

To implement the CSRF protection on the frontend using React, we need to make an API call to `/api/csrf-token` as described above.
```jsx
const [csrfToken, setCsrfToken] = useState("")

const getCSRFToken = () => {
	fetch('http://localhost:5000/api/csrf-token')
		.then(response => response.json())
		.then(data => {
			setCsrfToken(data['CSRFToken'])
		})
}

useEffect(() => {
	getCSRFToken()
}, [])
```

When the application loads the `csrfToken` value is set by making a call to the backend API at `/api/csrf-token`. Any request that needs to be protected by a CSRF Token will need to have the `CSRF-Token: csrfToken` header like so:
```jsx
const handleSubmit = () => {
	fetch('http://localhost:5000/api/support', {
		method: 'POST', credentials: "include", headers: {
			'Content-Type': 'application/json',
			'CSRF-Token': csrfToken
		}, body: JSON.stringify({
			url: urlChange
		})
	})
		.then(response => response.json())
		.then(data => {
			console.log(data)
		})
};
```

If the CSRF-Token header is not provided, then we get the following error on the backend:
```
ForbiddenError: invalid csrf token
```

---

**Summary**
Protecting your application from CSRF using React on the frontend and Express on the backend is not that difficult. Areas where developers could make mistakes is by not setting the CSRF-Token in the header when making a POST request. This could however be avoid by setting a default header for all POST requests like so:
```jsx
const getCSRFToken = async () => {
    const response = await axios.get('http://localhost:5000/api/csrf-token');
    axios.defaults.headers.post['X-CSRF-Token'] = response.data.CSRFToken;
 };
```

Other areas where developers could make mistakes is not setting CSRF protection on state changing function. In the example above we used `app.use(csrfProtection);` to implement CSRF protection on everything, but it is more common to only implement CSRF protection on functions that need it.
```js
app.post('/api/reset-password', csrfProtection, function (req, res) {
  res.json({ CSRFToken: req.csrfToken() });
})
```

**Vulnerability**
- https://fortbridge.co.uk/research/a-csrf-vulnerability-in-the-popular-csurf-package/


**References**
- https://www.stackhawk.com/blog/node-js-csrf-protection-guide-examples-and-how-to-enable-it/
- https://www.stackhawk.com/blog/react-csrf-protection-guide-examples-and-how-to-enable-it/

## ASP.NET MVC
ASP.NET MVC will automatically add a CSRF token to any form using a POST request. This can be seen in the field `__RequestVerificationToken`. Although ASP.NET automatically adds a CSRF token to the request, the developer will still need to *Validate* the token in the controller. This can be done by adding the `[ValidateAntiForgeryToken]` decorator.

*Example*
```cs
[HttpPost]
[ValidateAntiForgeryToken] // This decorator needs to be added for CSRF Protection
public IActionResult Index(IFormCollection fc){
	string res = fc[txtname];
	return View();
}
```

**References**
- https://www.stackhawk.com/blog/net-csrf-protection-guide-examples-and-how-to-enable/
- https://www.youtube.com/watch?v=3IQ3wgpjizc&t=998s&ab_channel=DotNetMastery

## ASP.NET Razor Pages
ASP.NET Razor Pages will automatically add a CSRF token to any form that is not a GET request. The difference between ASP.NET MVC CSRF protection and ASP.NET Razor Pages CSRF protection is that Razor Pages CSRF token's are automatically validated.

*Example*
```html
<form method="post">
    <p>Hello World!</p>
    <button type="submit">Go</button>
</form>
```

**References**
- https://exceptionnotfound.net/using-anti-forgery-tokens-in-asp-net-core-razor-pages/

## Ruby on Rails
Ruby on Rails applications will automatically add a CSRF token to any form that is not a GET request. The CSRF protection with contain a field called `authenticity_token` that will be sent along in the request. The CSRF protection can be seen implemented in the `applicaiton.html.erb` layout by the `<%= csrf_meta_tags %>` tag. 
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Csrf</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_importmap_tags %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```


**References**
- https://www.stackhawk.com/blog/rails-csrf-protection-guide/
- https://medium.com/rubyinside/a-deep-dive-into-csrf-protection-in-rails-19fa0a42c0ef#:~:text=Briefly%2C%20Cross%2DSite%20Request%20Forgery,their%20authenticity%20with%20each%20submission
- https://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf

