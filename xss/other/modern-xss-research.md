
# Products Tested
Framework  | Dangerous
---|---
React |  `dangerouslySetInnerHTML`
Vue |  `v-html`
Angular |  `[innerHtml]`
ASP.NET |  `@Html.Raw()`
Rails | `raw` or `html_safe`
Laravel | `echo`

**Summary**
All of the modern frameworks have put in place default protectection for XSS by automatically escaping any values before rendering them. All of these modern frameworks however contain at least one function that will disable the auto escape feature. This function may sometimes be used by the developer, but more commonly, these dangerous functions are being used in UI frameworks.

## 1) React
By Default, React will escape any values embedded in JSX before rendering them. 
```jsx
import React, { useState } from 'react';

const SearchBar = () => {
  const [searchText, setSearchText] = useState("");

  const handleChange = (event) => {
    setSearchText(event.target.value);
  }

  return (
	<div>
	  <input type="text" value={searchText} onChange={handleChange} />
	  <h2>You searched: {searchText}</h2>
	</div>
  );
}

export default SearchBar;
```

![[Pasted image 20230125085024.png]]

**Vulnerability 1) dangerouslySetInnerHTML**
`dangerouslySetInnerHTML` is React’s replacement for using `innerHTML` in the browser DOM.
```jsx
import React, { useState } from 'react';

const SearchBar = () => {
  const [searchText, setSearchText] = useState("");

  const handleChange = (event) => {
    setSearchText(event.target.value);
  }


  return (
	<div>
	  <input type="text" value={searchText} onChange={handleChange} />
	  <h2 dangerouslySetInnerHTML={{ "__html": "You Searched: " + searchText }} />
	</div>
  );
}

export default SearchBar;
```

![[Pasted image 20230125085835.png]]

**Vulnerability 2) XSS via a.href**
Having user input put directly into an href element can trigger an XSS.
> Note: The user will still need to click on the link in this senario.
```jsx
import React, { useState } from 'react';

const SearchBar = () => {
  const [searchText, setSearchText] = useState("");

  const handleChange = (event) => {
    setSearchText(event.target.value);
  }


  return (
	<div>
	  <input type="text" value={searchText} onChange={handleChange} />
	  <a href={searchText}>Website</a>
	</div>
  );
}

export default SearchBar;
```

![[Pasted image 20230125090519.png]]

**Source Code**
- The JavaScript file in React will be named `main.xxxxxxxx.js`
- I think that the custom code is appended at the bottom of the `main.xxxxxxxx.js` file
- The JavaScript `main.xxxxxxxx.js` file is obfuscated by default

**Other**
- The default testing port for React Applications is port 3000

**References**
- https://www.stackhawk.com/blog/react-xss-guide-examples-and-prevention/
- https://stackoverflow.com/questions/33644499/what-does-it-mean-when-they-say-react-is-xss-protected
- https://pragmaticwebsecurity.com/articles/spasecurity/react-xss-part3.html

## Vue
Just like React, Vue will also by default automatically escape any values embedded in JSX before rendering them. 
```
h1>{{ searchTerm }}</h1>
```

If `searchTerm` contained:
```
<script>alert("hacked")</script>
```

then it would be escaped to the following HTML:
```
&lt;script&gt;alert(&quot;hi&quot;)&lt;/script&gt;
```
thus preventing the script injection.

```jsx
<template>
  <div>
    <input v-model="searchTerm" @keyup.enter="search"/>
    <h2>{{ searchTerm }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: ''
    }
  }
}
</script>
```

![[Pasted image 20230125095612.png]]

**Vulnerability 1) v-html**
`v-html` is Vue's replacement for using `innerHTML` in the browser DOM.
```jsx
<template>
  <div>
    <input v-model="searchTerm" @keyup.enter="search"/>
    <h2 v-html="searchTerm"></h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: ''
    }
  }
}
</script>
```

![[Pasted image 20230125101804.png]]


**Vulnerability 2) XSS via a.href**


**Vulnerability 3) CSTI**
Unlike React, Vue can also be used as a templating engine making it vulnerable to CSTI.
```js
const express = require('express');
const escapeHTML = require('escape-html');

const app = express();

app.get('/', (req, res) => {
  res.set('Content-Type', 'text/html');
  const name = req.query.name
  res.status(200).send(`<!DOCTYPE html>
<html>
<body>
  <div id="app">
    <h1>Hello ?name=${escapeHTML(name)}</h1>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>
  <script>
      new Vue({
        el: '#app'
      });
  </script>
</body>
</html>`);
});

module.exports = app;
```

**Source Code**
- The JavaScript file in Vue will be named `index-xxxxxxxx.js`
- The JavaScript `index-xxxxxxxx.js` file is obfuscated by default

**Other**
The default testing port for Vue Applications is port 5173

**References**
- https://vuejs.org/guide/best-practices/security.html#potential-dangers
- https://dev-academy.com/vue-xss/
- https://sqreen.github.io/VueXSSDemo/#/
- https://vue-client-side-template-injection-example.azu.vercel.app/?name=test
- https://www.youtube.com/watch?v=-_7uL7l0qZk&ab_channel=intigriti

## Angular
Angular will also by default automatically escape any values before rendering them. 
![[Pasted image 20230130112439.png]]
*View*
```html
<div>
  <h1>{{title}}</h1>
  <input type="text" #box (keyup)="getValue(box.value)">
  <h2>{{userInput}}</h2>
</div>
```

*Component*
```js
export class AppComponent {
  title = 'Angular XSS';
  userInput: string = "";
  getValue(val:string){
    this.userInput = val
  }
}
```

**Vulnerability 1) innerHtml & Dom Sanitizer**
We can use the `innerHtml` flag to tell Angular to not sanitize input:
```html
<div>
  <h1>{{title}}</h1>
  <input type="text" #box (keyup)="getValue(box.value)">
  <h2 [innerHtml]="userInput"></h2>
</div>
```

This will result in an attacker being able to perform HTML injection, but not XSS. Angular has protections inplace to sanitize malicious input in `innerHtml` is in use.
![[Pasted image 20230130113817.png]]
 To get XSS, the developer will need to also add the `bypassSecurityTrustScript` method and change the input type to `SafeScript`
```js
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

export class AppComponent {
  title = 'Angular XSS';
  userInput: SafeHtml = "";

  constructor(protected sanitizer: DomSanitizer) {}

  getValue(val:any){
    this.userInput = this.sanitizer.bypassSecurityTrustHtml(val)
  }
}
```

**Other**
- The default testing port for Angular Applications is port 4200

**References**
- https://www.stackhawk.com/blog/angular-xss-guide-examples-and-prevention/
- https://angular.io/guide/security
- https://medium.com/@swarnakishore/angular-safe-pipe-implementation-to-bypass-domsanitizer-stripping-out-content-c1bf0f1cc36b

## ASP.NET
The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.

*View*
```html
@page
@model WebApplication1.Pages.SearchModel
@{
}

<div>
    <form class="text-center" method="post">
        <input type="text" name="query" placeholder="Search...">
        <button type="submit">Search</button>
    </form>
    <h2 class="text-center">@Model.SearchResult</h2>
</div>
```

*Controller*
```cs
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace WebApplication1.Pages
{
    public class SearchModel : PageModel
    {

        public string SearchResult = "Test";

        public void OnPost(string query)
        {
            System.Diagnostics.Debug.WriteLine("Hello World");
            SearchResult = query;
        }
    }
}
```

![[Pasted image 20230125155634.png]]

**Vulnerability 1) @Html.Raw()**
Using the `@Html.Raw()` method will cause ASP.NET to not do an auto escaping.
```html
@page
@model WebApplication1.Pages.SearchModel
@{
}

<div>
    <form class="text-center" method="post">
        <input type="text" name="query" placeholder="Search...">
        <button type="submit">Search</button>
    </form>
    <h2 class="text-center">@Html.Raw(@Model.SearchResult)</h2>
</div>
```

![[Pasted image 20230125161029.png]]

**Vulnerability 2) href**
```cs
@page
@model WebApplication1.Pages.SearchModel
@{
}

<div>
    <form class="text-center" method="post">
        <input type="text" name="query" placeholder="Search...">
        <button type="submit">Search</button>
    </form>
    <a href=@Model.SearchResult>Website</a>
</div>
```

> Note: You will still need to click on the website link in order for the XSS to trigger.
![[Pasted image 20230125161507.png]]

**Source Code**
- ASP.NET code is not obfuscated by default.

**Other**
The default testing port for ASP.NET Applications is port 7126

**References**
- https://www.stackhawk.com/blog/net-xss-examples-and-prevention/
- https://www.youtube.com/watch?v=eru2emiqow0&ab_channel=freeCodeCamp.org


## Ruby on Rails
By Default, Ruby on Rails will escape any data automatically.
![[Pasted image 20230126085245.png]]

*View*
```ruby
<%= form_with method: :get do |form| %>
    <%= form.label :query, "Search for:" %>
    <%= form.text_field :query %>
    <%= form.submit "Search" %>
  <% end %>
  <h2>You Searched: <%= @result %> </h2>
```

*Controller*
```ruby
class HomeController < ApplicationController
  def search
    p params[:query]
    @result = params[:query]
  end
end
```


**Vulnerability 1) raw**
Using the `raw` helper will disable ruby on rails from escaping the data.
```ruby
<%= form_with method: :get do |form| %>
    <%= form.label :query, "Search for:" %>
    <%= form.text_field :query %>
    <%= form.submit "Search" %>
  <% end %>
  <h2>You Searched: <%= raw @result %> </h2>
```

![[Pasted image 20230126085648.png]]

**Vulnerability 2) html_safe**
Using the `html_safe` helper will disable ruby on rails from escaping the data.
```ruby
<%= form_with method: :get do |form| %>
    <%= form.label :query, "Search for:" %>
    <%= form.text_field :query %>
    <%= form.submit "Search" %>
  <% end %>
  <h2>You Searched: <%= @result.html_safe %> </h2>
```

![[Pasted image 20230126085915.png]]

**Vulnerability 3) href**
Passing user supplied data to a `link_to` tag can result in XSS.
```ruby
<%= form_with method: :get do |form| %>
    <%= form.label :query, "Search for:" %>
    <%= form.text_field :query %>
    <%= form.submit "Search" %>
  <% end %>
  <%= link_to "Website", @result %>
```

![[Pasted image 20230126090452.png]]

**References**
- https://www.stackhawk.com/blog/rails-xss-examples-and-prevention/


## Spring Boot
Sping Boot is used as a back-end framework for creating an API to communicate with the front-end. The front-end is usually React, Vue or Angular. Spring Boot applications may also commonly use a templating engine such as Thymeleaf, Apache Freemarker, Mustache, or Groovy Templates. If the application is using a templating engine then this opens the application to SSTI vulnerabilities. 

## Django
Django is typically used as a back-end framework for creating an API to communicate with the front-end. The front-end is usually React, Vue or Angular. Django application may also templating engines, but not as common. If the application is using a templating engine then this opens the application to SSTI vulnerabilities. 

**Other**
- The default testing port for Django Applications is port 8000

## Laravel
By Default, Laravel will escape any data automatically.
![[Pasted image 20230130095845.png]]

*Controller*
```php
class SearchController extends Controller
{
    public function submit(Request $request)
    {
        $search = $request->input('search');

        // Process form data
        return view('welcome', ['search' => $search]);
    }
}
```

*View*
```html
<html>
    <body class="antialiased">
        <div>
            <h1>XSS Laravel</h1>
            <form action="/submit" method="post">
                @csrf
                <input type="text" name="search">
                <input type="submit" value="Submit">
            </form>
            <h2>You Searched: {{ $search }}</h2>
        </div> 
    </body>
</html>
```


**Vulnerability 1) echo**
The example above uses the blade templating engine to render the user's input on the screen. The blade templating engine will automatically escape any HTML characters. Using the `echo` command to render user input can result in XSS as the input will not be escaped.

*Controller*
```php
class SearchController extends Controller
{
    public function submit(Request $request)
    {
        $search = $request->input('search');

        echo $search;
    }
}
```

The following XSS vulnerability can be prevented by using the `htmlspecialchars` function:
```php
class SearchController extends Controller
{
    public function submit(Request $request)
    {
        $search = $request->input('search');

        echo htmlspecialchars($search);
    }
}
```

**Vulnerability 2) href**
Inserting user input into an `href` tag will result in XSS.

*View*
```html
<html>
    <body class="antialiased">
        <div>
            <h1>XSS Laravel</h1>
            <form action="/submit" method="post">
                @csrf
                <input type="text" name="search">
                <input type="submit" value="Submit">
            </form>
            <a href={{$search}}>Website</a>
        </div> 
    </body>
</html>
```


**References**
- https://www.stackhawk.com/blog/laravel-xss/

# Writeup & Reports
All of these reports were performed on modern frameworks.
- https://www.youtube.com/watch?v=HdocPEp9oZs&ab_channel=HACKERFUDDI

# Takeaways
Before starting this research I thought that React was the only JavaScript framework that automatically encoded characters needed for XSS. Upon doing the research this indeed is not the case as every JavaScript framework does this. My key takeaway from this research is to search for XSS in old technologies that don't have automatic encoding in place. Also to look for XSS in markdown fields and in areas where your input is being inserted into an href tag. 
