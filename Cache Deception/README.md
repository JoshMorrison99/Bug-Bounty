**Step 1**
Search for parts of the application where sensitive information is being reflected in the response, such as session cookies, email, etc.

**Step 2**
Try to cache the sensitive information
- [ ] Check for cachable Apache path normalization `GET //blog`
- [ ] Check for cachable Nginx path normalization `GET /blog%3F`
- [ ] Check for cachable PHP path normalization `GET /blog.php/xyz`
- [ ] Check for cachable .NET path normalization `GET /blog(A(xyz))/`
- [ ] Check for cachable extension
	- [ ] `GET /profile/.js?cb=1`
	- [ ] `GET /profile/.css?cb=1`
	- [ ] `GET /profile/.png?cb=1`
