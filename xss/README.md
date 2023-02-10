 My methodology for finding XSS is to hunt for applications that are not being protected by a framework or CMS. I do this because frameworks will automatically escape HTML characters before rendering them. Here is a list of some popular frameworks:
 - React
 - Vue
 - Angular
 - ASP.NET
 - Laravel
 - Ruby on Rails

Before doing any research I thought React was the only framework that has automatic XSS protection. It turns out every framework has automatic XSS protection. In fact, it is actually quite difficult for the developer to accidentally make the application vunlerable to XSS. All of these modern frameworks however contain at least one function that will disable the auto escape feature. 

**Summary**
Framework  | Dangerous
---|---
React |  `dangerouslySetInnerHTML`
Vue |  `v-html`
Angular |  `[innerHtml]`
ASP.NET |  `@Html.Raw()`
Rails | `raw` or `html_safe`
Laravel | `echo`

Disabling the autoescaping feature is commonly seen in functionality where users have the ability to edit HTML such as a blog website. Other than disabling the auto escape feature, these frameworks are also all vulnerable to XSS if the user's input is passed into an href tag like so: `<a href=XSS>Website</a>`. This kind of functionality is commonly seen in user's profile where the website allows the user to add a link to their own website.

To hunt for applications that are not using frameworks, I will use a combination of Google Dorking and command line tools to get URLs with certain extentions. If a webpage has one of the following extensions, then this means that the developer had to go out of their way to prevent XSS.

Here are the Google Dorks I use to hunt for XSS:
```
site:*.website.com ext:php
site:*.website.com ext:asp
site:*.website.com ext:aspx
site:*.website.com ext:jsp
site:*.website.com ext:jspx
site:*.website.com ext:do
site:*.website.com ext:action
```

The Google Dorks are great, but it will miss a lot of websites that have "vulnerable extension". In addtion to Google Dorking I will also use the GAU tool and Katana to get URLs. I will then filter those URLs for "vulnerable extensions".
```
cat urls.txt | grep '\.php' | tee php.txt
cat urls.txt | grep '\.do$' | tee do.txt
cat urls.txt | grep '\.asp' | tee asp.txt
cat urls.txt | grep '\.jsp' | tee jsp.txt
cat urls.txt | grep '\.action' | tee action.txt
```

I will generally manually check each site as I can perform more thorough testing on parameters, headers and authenticated areas. I do however also automate for finding low hanging fruits by using the KXSS tool.
```
cat urls.txt | ./kxss
```

