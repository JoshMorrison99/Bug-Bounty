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
