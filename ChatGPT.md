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
