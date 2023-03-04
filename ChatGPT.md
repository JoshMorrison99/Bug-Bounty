**Question 1**
```
why do some websites implement a hashtag before a directory like so: /#/login
```

Websites that implement a hashtag before a directory like "/#/login" are likely using client-side routing or the so-called hash routing.

In traditional server-side routing, when a user requests a URL, the server responds with the appropriate content based on the requested URL. However, in client-side routing, the client (i.e. the browser) handles the routing internally without requesting content from the server.

Client-side routing can be useful in single-page applications (SPAs) where the server only provides the initial HTML and JavaScript, and the rest of the content is loaded dynamically via JavaScript. In such cases, client-side routing can be used to update the browser's URL and history state without actually requesting content from the server.

The hashtag (or hashbang, denoted by #!) before the directory is a technique used to implement client-side routing. When a user clicks a link with a hashtag, the browser updates the URL without actually requesting content from the server. The JavaScript code running on the page can then read the new URL and display the appropriate content based on the requested route.

For example, in an SPA, clicking a link with "/#/login" might trigger a JavaScript function that displays a login form in the page's content area without requesting a new page from the server.

It's worth noting that this technique is not considered best practice in modern web development. Instead, the HTML5 History API is typically used to update the browser's URL and history state without using hashtags. This allows for more semantic URLs and better SEO.

<br/>

**Question 2**
```
why would a web developer give the session cookie a value of SameSite: none 
```

A web developer might give the session cookie a value of SameSite: none for scenarios where the website needs to use third-party cookies or perform cross-site requests. The SameSite attribute on a cookie defines the cookie's cross-site access, indicating whether the cookie should be sent along with requests initiated from third-party websites.

By setting the SameSite attribute to "none," the cookie can be accessed by other websites, allowing for a seamless user experience, such as when an embedded payment gateway needs to be accessed from a different domain. If the SameSite attribute is not set to "none," then cross-site requests will fail, which can cause issues for the user and potentially break functionality on the website.

However, it's important to note that setting SameSite: none can also introduce some security concerns as it makes the cookie more vulnerable to Cross-Site Request Forgery (CSRF) attacks. As a result, it is recommended that developers implement additional security measures such as implementing anti-CSRF tokens to mitigate these risks.

<br/>

**Question 3**
```
is IIS 10 vulnerable to shortname scanning
```
IIS 10, like its predecessors, is vulnerable to shortname scanning attacks, which is also known as 8.3 filename enumeration or file/folder name bruteforcing. This vulnerability stems from the legacy support for the 8.3 naming convention used in older versions of Windows.

Shortname scanning is a technique that allows an attacker to map the directory structure of a web server by guessing short filenames that match long filenames. By brute-forcing these short filenames, an attacker can determine the existence and location of files and directories that may not be otherwise accessible. This vulnerability can be used for various types of attacks, such as reconnaissance, information disclosure, and potentially even remote code execution.

To mitigate the risk of shortname scanning attacks, Microsoft recommends disabling 8.3 filename generation for all volumes that contain web content. This can be done using the command line tool "fsutil". Additionally, keeping IIS up to date with the latest security patches and following best practices for web server security can help reduce the risk of exploitation.

<br/>

**Question 4**
```
why do some websites have an autodiscover subdomain
```

The "autodiscover" subdomain is commonly used by email clients and servers to automatically discover the configuration settings for a user's email account. This is usually done to simplify the setup process for email accounts, as users don't need to manually input server settings like incoming and outgoing mail servers, ports, and encryption settings.

When an email client or server connects to the "autodiscover" subdomain, it sends a request to the server hosting that subdomain, asking for the configuration settings for a specific email address. The server then responds with the necessary settings, which the email client or server can use to automatically configure the account.

The use of an "autodiscover" subdomain is not required for email setup, but it can be a helpful and convenient feature for users who are not familiar with email server settings or who want to save time during the setup process.

<br/>

**Question 5**
```
why do some websites have an lyncdiscover subdomain
```

The "lyncdiscover" subdomain is used by Microsoft Lync and Skype for Business applications to automatically discover the configuration settings for a user's Lync or Skype for Business account. This subdomain works in a similar way to the "autodiscover" subdomain used for email accounts.

When a user signs in to Lync or Skype for Business, the application uses the "lyncdiscover" subdomain to request the necessary configuration settings from the server. The server responds with the settings, which the application then uses to connect to the Lync or Skype for Business server and communicate with other users.

The "lyncdiscover" subdomain is not required for Lync or Skype for Business to function, but it can simplify the setup process for users and make it easier for them to connect to the server. It can also be helpful for administrators who need to configure multiple Lync or Skype for Business clients, as they can use the subdomain to automatically distribute the necessary settings to all clients.

<br/>

**Question 6**
```
why would a website use the X-Forwarded-Host header?
```
A website might use the X-Forwarded-Host header for several reasons, but the most common reason is to properly handle requests that are forwarded from a reverse proxy or a load balancer.

When a website is hosted behind a reverse proxy or load balancer, the request that comes to the website's server might include the IP address or hostname of the proxy server instead of the actual IP address or hostname of the client. This is because the client's request is intercepted and forwarded by the proxy server. In order for the website to properly handle the request, it needs to know the original hostname or IP address of the client that made the request.

The X-Forwarded-Host header allows the proxy server to pass along the original hostname of the client in the request. The website can then use this information to properly handle the request and respond with the correct content or redirect the user to the correct URL.

Additionally, using the X-Forwarded-Host header can help prevent security vulnerabilities such as "Host Header Attacks" which can occur when an attacker sends a request with a fake hostname to a server, tricking it to respond with sensitive information.

Overall, the X-Forwarded-Host header is a way to maintain the proper communication between client and server, and improve security by ensuring the authenticity of the incoming requests.

<br/>

**Question 7**
```
why would a website want to know my original ip address
```

There are several reasons why a website might want to know your original IP address:

Geolocation: By knowing your IP address, a website can determine your approximate location. This can be useful for providing location-specific content or services.

Security: A website may use your IP address to help identify potential security threats, such as malicious users or bots.

Analytics: Websites often use IP addresses to track website traffic and usage patterns. This information can be used to improve the website's performance or to target advertising to specific users.

Compliance: In some cases, websites may need to collect and store IP addresses in order to comply with legal or regulatory requirements.

It's worth noting that most websites will not be able to see your original IP address if you are using a VPN (Virtual Private Network) or other anonymity tool. In this case, the website will only be able to see the IP address of the VPN server you are connected to.

<br/>

**Question 8**
```
what is the difference between the x-forwarded-for and x-real-ip?
```

The X-Forwarded-For and X-Real-IP headers are both used to pass the original client IP address to a backend server that is behind a proxy or load balancer, but they differ in their implementation and usage.

The X-Forwarded-For header is a standard HTTP header that is used to pass the original client IP address to a backend server that is behind a proxy or load balancer. This header is added by the proxy or load balancer to the incoming request, containing the original IP address of the client. The backend server can then use this header to determine the original IP address of the client, in case it needs to perform IP-based access control or logging. However, the X-Forwarded-For header can be easily spoofed, as it can be set to any value by a malicious client or intermediate proxy.

The X-Real-IP header, on the other hand, is a non-standard header that is used by the Nginx web server to pass the original client IP address to a backend server. This header is added by Nginx itself, based on the value of the real_ip_header directive in the Nginx configuration file. Unlike X-Forwarded-For, the X-Real-IP header cannot be easily spoofed, as it is added by a trusted source (Nginx) and is not directly modifiable by an intermediate proxy.

In summary, the X-Forwarded-For header is a standard header that is widely used in the industry to pass the original client IP address, but it can be spoofed. The X-Real-IP header, on the other hand, is a non-standard header that is specific to Nginx, but it provides a more secure way to pass the original client IP address, as it cannot be easily spoofed.

<br/>

**Question 9**
```
what is the difference between x-forwarded-for and x-forwarded-host?
```

The X-Forwarded-For and X-Forwarded-Host headers serve different purposes in an HTTP request.

The X-Forwarded-For header is a standard HTTP header that is used to pass the original client IP address to a backend server that is behind a proxy or load balancer. This header is added by the proxy or load balancer to the incoming request, containing the original IP address of the client. The backend server can then use this header to determine the original IP address of the client, in case it needs to perform IP-based access control or logging.

The X-Forwarded-Host header, on the other hand, is not a standard HTTP header, but rather a non-standard header that is used to pass the original host name or IP address of a client to a backend server that is behind a proxy or load balancer. This header is added by the proxy or load balancer to the incoming request, containing the original host name or IP address of the client. The backend server can then use this header to determine the original host name or IP address of the client, in case it needs to generate links or perform other operations based on the original client request.

The main difference between the two headers is that X-Forwarded-For is used to pass the client IP address, while X-Forwarded-Host is used to pass the client host name or IP address. They are often used together to provide a complete picture of the original client request, including the original IP address and host name.

<br/>

**Question 10**
```
what is the difference between x-forwarded-host and the Host header?
```

The Host header and the X-Forwarded-Host header serve different purposes in an HTTP request.

The Host header is a mandatory HTTP request header that specifies the host name or IP address of the server that the client is requesting. This header is used by the server to determine which virtual host should handle the request, in case the server is hosting multiple virtual hosts. The Host header is sent by the client in the initial request, and it contains the original host name or IP address that the client used to make the request.

The X-Forwarded-Host header, on the other hand, is not a standard HTTP header, but rather a non-standard header that is used to pass the original host name or IP address of a client to a backend server that is behind a proxy or load balancer. This header is added by the proxy or load balancer to the incoming request, containing the original host name or IP address of the client.

The main difference between the two headers is that the Host header is mandatory and must be included in every HTTP request, while the X-Forwarded-Host header is optional and only needs to be included if the client is behind a proxy or load balancer.

Another difference is that the Host header is typically used by the server to determine which virtual host should handle the request, while the X-Forwarded-Host header is used by the backend server to determine the original host name or IP address of the client, in case it needs to generate links or perform other operations based on the original client request.

<br/>

**Question 11**
```
why would a website want to know my original host name
```

A website may want to know your original host name for a few reasons:

Security purposes: Your original host name can help the website identify and authenticate your device, which can help prevent unauthorized access and protect against potential security threats.

Personalization: Knowing your original host name can allow the website to provide a more personalized experience. For example, the website may use this information to remember your preferences or to provide customized recommendations.

Analytics: Websites often use data analytics tools to analyze user behavior and improve their website. Knowing your original host name can help them understand where their users are coming from and how they interact with the website.

Troubleshooting: If you are experiencing issues with the website, providing your original host name can help the website's support team diagnose and fix the problem.

Overall, providing your original host name can help the website provide a better and more secure user experience. However, it is always important to be cautious about sharing personal information online and to only provide information to websites that you trust.

<br/>

**Question 12**
```
How come when I send: "Location: test.com%0D%0A" it is being encoded to "Set-Cookie: location=test.com%250D%250A; Path=/" in the response. I am not doing any protection against CRLF in my express applicaiton
```


The reason why your Location header value is being encoded to test.com%250D%250A instead of being interpreted as a CRLF sequence is because Express.js automatically encodes any special characters in HTTP header values to prevent header injection attacks.

In this case, the %0D%0A sequence is the URL-encoded representation of the CRLF characters, and Express.js is encoding it to %250D%250A, which is the URL-encoded representation of the %0D%0A sequence.

By automatically encoding special characters in HTTP header values, Express.js is helping to prevent CRLF injection attacks, which are a type of vulnerability where an attacker is able to inject CRLF characters into the response headers or response body of an HTTP response.

To prevent automatic encoding of special characters in HTTP header values, you can set the http.IncomingMessage object's rawHeaders property to an array of unencoded header values. However, this is not recommended as it can open up the application to security vulnerabilities. Instead, it is recommended to properly sanitize and validate any user input that is used in response headers or response body to prevent CRLF injection attacks.

<br/>
**Question 13**
```
why would a developer implement a method override parameter such as _method=POST?
```

A developer may implement a method override parameter such as "_method=POST" in order to support RESTful web applications that use HTTP methods such as GET, POST, PUT, DELETE, etc.

In some cases, a web application may need to perform an action that is not supported by the HTTP method currently being used. For example, a web application may need to update a resource using a PUT request, but the client's browser only supports GET and POST requests. In such cases, the developer can use the "_method" parameter to specify the desired HTTP method.

By including the "_method" parameter with a value of "POST" (or another supported HTTP method), the developer can instruct the web application to treat the request as if it were a PUT (or another desired method) request instead. This is often achieved by configuring the server to look for the "_method" parameter in the request and override the HTTP method accordingly.

Overall, using the "_method" parameter can be a useful technique for developers to support a wider range of HTTP methods in their web applications, even if the client's browser does not support them directly.
