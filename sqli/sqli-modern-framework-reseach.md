
**Summary**
Each framework has their own ORM (Object-Relational Mapping) which provides a high-level, abstract API for accessing the database, and it automatically escapes any user-supplied data to prevent SQL injection attacks. SQL injections can however creep into the application when the developer decides to not use the ORM and instead write raw SQL queries. In this case, the developer can still be protected from SQL injection by using parameterized queries. SQL injection will only occur when the developer decides to not use the ORM or paramerterized queries.

*ORM*
```js
const user = User.object.all()
```

*Parameterized/Prepared Query*
```js
const user = await pool.query('SELECT * FROM users WHERE username = $1', [username]);
```

*Raw Query*
```js
const user = await pool.query("SELECT * FROM users WHERE username = '" + username + "'");
```

**What is an ORM?**
Object-Relational Mapping (ORM) is a technique that allows a software application to interact with a database in an abstract and object-oriented way. Instead of writing raw SQL statements to interact with the database, ORM provides a higher-level, object-oriented API that allows you to interact with the database using objects and methods that correspond to database tables and operations.

## Express 
```js
const express = require('express')
const app = express()
const pool = require("./db");
var cors = require('cors')

const port = 3001

app.use(cors())

// Query users
app.get('/query', async (req, res) => {
    try {
        const { username } = req.query
        const user = await pool.query("SELECT * FROM users WHERE username = '" + username + "'");
        console.log(user.rows)
    } catch (err) {
        console.error(err.message)
    }
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```

**Vulnerability 1) User Controlled Variable without Sanitization**
The query below is vulnerable to SQL injection attack.
```js
const user = await pool.query("SELECT * FROM users WHERE username = '" + username + "'");
```

The following payload can be used to retrive all users from the database.
```sql
' OR 1=1-- -
```

```sql
SELECT * FROM users WHERE username = '' OR 1=1-- -'
```

**Vulnerability 2) NodeJS SQL Injection in mysqljs Library**
https://flattsecurity.medium.com/finding-an-unseen-sql-injection-by-bypassing-escape-functions-in-mysqljs-mysql-90b27f6542b4

*Payload*
```json
username=admin&password[password]=1
```


**Prevention 1) Prepared Statements**
```js
const express = require('express')
const app = express()
const pool = require("./db");
var cors = require('cors')

const port = 3001

app.use(cors())

// Query users
app.get('/query', async (req, res) => {
    try {
        const { username } = req.query
        const user = await pool.query('SELECT * FROM users WHERE username = $1', [username]);
        res.json(user.rows)
    } catch (err) {
        console.error(err.message)
    }
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```

## 2) Django
The code below takes a value from the query parameter `http://website.com?q=hello`. The value in the query parameter is then used to query to database.
```python
def home_view(request):
    query = request.GET.get('q')
    products = Product.objects.filter(title__icontains=query)
    return render(request, "home.html", {'products': products})
```

In the senario above, the applicaiton is using Django's ORM (Object-Relational Mapping) to interact with the database: The Django ORM provides a high-level, abstract API for accessing the database, and it automatically escapes any user-supplied data to prevent SQL injection attacks.

In Django, if we wanted to get all Students we would use the following query:
```python
Student.object.all()
```

In raw SQL, we would use the following query:
```sql
Select * from Student
```

There are some cases where raw SQL queries may be necessary, for example:
1.  **Complex queries**: If you need to execute a complex query that cannot be easily expressed using the Django ORM, you may need to use raw SQL.
2.  **Performance optimization:** In some cases, using raw SQL can result in better performance than using the Django ORM, especially for complex or resource-intensive operations.
3.  **Database-specific features:** If you need to use a database-specific feature that is not supported by the Django ORM, you may need to use raw SQL.

In these cases, you should use the `django.db.connection.cursor()` method to create a cursor object, and use the cursor object to execute the raw SQL query. This method automatically escapes any user-supplied data to prevent SQL injection attacks.

**Vulnerability 1) Raw Query**
```python
def home_view(request):
    query = request.GET.get('q')
    products = Product.objects.raw("SELECT * FROM sqli_product WHERE title='" + query + "'")
    return render(request, "home.html", {'products': products})
```


## 3) ASP.NET
The Entity Framework includes an ORM to perform database queries. 
```cs
public User GetUser(string username)
	{
		return _context.Users.Where(x => x.UserName == username).FirstOrDefault();
	}
```

If the developer for some reason decides to use raw SQL, then the developer can protect from SQL injection by using parameterized queries.
```cs
public User GetUserRawParameterized(string username)
	{
		return _context.Users.FromSqlRaw("SELECT * FROM Users WHERE username = {0}", username).FirstOrDefault();
	}
```

If the developer for some reason decides to not use the ORM or parameterized queries, then the application may be vulnerable to SQL injection.
```cs
public User GetUserRaw(string username)
	{
		var query = "SELECT * FROM Users WHERE username = '" + username + "'";
		return _context.Users.FromSqlRaw(query).FirstOrDefault();
	}
```

## 4) Ruby on Rails
The ORM in Ruby on Rails is called Active Record. It provides a convenient and efficient way to interact with databases using an object-oriented paradigm, allowing you to work with data stored in your database as objects, rather than writing raw SQL.
```ruby
class SqliController < ApplicationController
    def index
        @result = params[:query]
        @users = User.find_by(username: @result)
    end
end
```


The Rails application is vulnerable if the developer uses a raw query like so:
```ruby
class SqliController < ApplicationController
    def index
        @result = params[:query]
        @users = User.connection.execute("SELECT * FROM users WHERE username='" + @result + "'")
    end
end
```

## 5) Spring Boot
Spring Boot provides support for several ORM technologies, including JPA (Java Persistence API), Hibernate, and MyBatis.

JPA is a Java specification for accessing, persisting, and managing data between Java objects and a relational database. Hibernate is a popular implementation of JPA that provides additional features and enhancements. MyBatis is another popular ORM framework that provides a different approach to mapping between data and objects.

## 6) Laravel
Laravel uses the Eloquent ORM (Object-Relational Mapping) for working with databases. The Eloquent ORM provides an elegant and simple way to interact with databases using an object-oriented syntax, without writing complex SQL queries.

Laravel protects against SQL injection attacks by using prepared statements. When you use the Eloquent ORM to interact with the database, it automatically escapes any user-supplied data before including it in a SQL query, which helps to prevent SQL injection attacks.

For example, if you want to retrieve data from the database using a specific condition, you can do so using the following code in Laravel:
```php
$users = User::where('name', '=', $name)->get();
```


In this example, `$name` is a user-supplied input and Laravel automatically escapes it before including it in the SQL query. This helps to prevent SQL injection attacks by ensuring that any malicious input is treated as plain text instead of executable code.
