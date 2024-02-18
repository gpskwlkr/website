+++
title = 'Entity Framework 5 Rookie Traps'
date = 2023-09-26
draft = false 
showReadingTime = true
showPublicationDate = true
comments = true
tags = ["database", "dotnet", "backend"]
categories = ["Database Development","Back End"]
keywords = ["Entity Framework", "EF", ".NET", "beginner developer", "job requirements", "guides", "benefits", "caveats", "DB", "Code-First", "nvarchar(max)", "performance", "indexes", "data retrieval", "Lazy Loading", "Eager Loading", "N+1 problem", "Migrations", "CLI", "SQL", "JOIN operations", "Security concerns", "RAW SQL", "SQLParameter class", "DbContext", "Dapper", "SQL Injection attacks"]
+++

{{<img src="/images/entity-framework.webp" align="center" alt="Entity Framework logo">}}<br />

I’m pretty sure that everyone who works with C#/.NET has heard at least once about Entity Framework (EF). Whether it’s good or bad, it’s used widely and it will benefit you to know how to work with it. <!--more--> But what happens when a beginner developer enters the .NET world and sees the picture where EF is promoted everywhere? All of the job requirements include it and you can find plenty of guides on it. We can’t ignore that EF gives you a lot of benefits, but some caveats may be not so visible for a beginner developer and in this article, I’d like to introduce them to you.

## 1. Control over the DB while doing Code-First

**Scenario:**

> Imagine having a simple User model for your database.

Like the one below:

```csharp
public class User
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }
}

// DbContext declaration
// ...
public DbSet<User> Users { get; set; }
```

And since you’re a beginner you might think it’s completely fine, but this is what you end up having in your database:

{{<img src="/images/nvarchar-max.webp" align="center" alt="Image showing nvarchar(max) in table">}}

As you might see, if you don’t specify any length to your string fields, EF will default it to nvarchar(max) and you might be asking, so what? What’s wrong with that? I have plenty of reasons why you should never use it unless it’s really necessary for business needs:

- nvarchar(max) fields can potentially store up to 2GB of data for each row. So when the data is queried, the database might allocate memory based on the potential size of the column, even if only a fraction of it is used, which leads to inefficient memory usage.
- You can’t apply any traditional indexes to nvarchar(max) columns. You could still use full-text indexes or indexed views in some cases, but it will still limit your performance optimization options.
- With all of that said above, data retrieval could be slow because of nvarchar(max) , especially when they are being used for JOIN operations

So, including all cons of nvarchar(max) what you should consider is refactoring your DB model to be like:

```csharp
public class User
{
    public int Id { get; set; }
    // if you want FirstName to remain nvarchar type
    // but limit it's length
    [MaxLength(200)]
    public string FirstName { get; set; }
    // or even better
    // if you only allow alphanumeric characters
    // or special characters & whitespace
    [Column(TypeName = "varchar(200)")]
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }
}
```

Always check your model for having your string fields limited by length or column data type.

## 2. Lazy Loading vs. Eager Loading

Many beginners might have spotted some performance problems with their EF queries. Entity Framework is not forcing you to use Lazy Loading or Eager Loading, you get to choose what to do. But first, before we talk about what are the pros and cons of each of them, let’s dive into how they work.

**Lazy Loading**

Lazy Loading is a feature that allows EF to query the data on an as-needed basis.

**Scenario:**

> Imagine having a blog where users should be allowed to have many posts.

So you build some simple models like this:

```csharp
public class User
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }

    public ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
}
```

And later on in your service, you query the user with all the posts

```csharp
// method declaration
// ...
var users = _dbContext.Users.ToList();
foreach (var user in users)
{
    foreach (var post in user.Posts)
    {
      Console.WriteLine(post.Title);
    }
}
```

You might be thinking that it’s cool to load data only when you need it, so you’re not overloading your database, but what happens is that if you have 1 user who has 100 posts on your blog, you’re making 101 separate queries, 1 to query the user and 100 others to query all the posts.

> Often referred to as N+1 problem.

That’s when Eager Loading comes in to save your database!

**Eager Loading**

On the other side, Eager Loading is a feature that allows EF to query all the required data in a single query and map it as your model declares. In terms of code, you’ve got the new .Include() method.

```csharp
var users = _dbContext.Users
                      .Include(u => u.Posts)
                      .ToList();
```

And that’s it. Now you’ve queried all the users with all of their posts in a single query which is a more efficient approach while working with big data models as blog posts.

## 3. Ignoring Database Migrations

EF provides a mechanism called “Migrations” to help you manage changes to the database schema. You add a new migration each time you’ve changed a database entity model and EF generates a set of commands it then executes to update the database table according to your model. Beginners often forget to apply their migrations to the database which can lead to having certain errors later on. If you’re someone who loves working from CLI, execute these commands after each change to keep your database in sync with your entities.

```
dotnet ef migrations add nameOfMigration
dotnet ef database update
```

> Make sure you’ve installed [EF Tools](https://learn.microsoft.com/en-us/ef/core/cli/dotnet) before executing these commands.

## 4. Not keeping track of SQL generated by EF

EF abstracts the SQL generation process, which can be a double-edged sword. While it allows developers to work without worrying about SQL, it can lead to inefficient or unexpected SQL code generated.

Let’s have kinda same example and see how EF generates SQL code and how can we do it ourselves.

**Scenario:**

> Imagine you have a Users table and a Purchases table. You want to fetch all users who have made more than 5 purchases.

Using EF you might write something like this:

```csharp
var usersWithMoreThanFivePurchases = context.Users
    .Where(u => u.Purchases.Count() > 5)
    .ToList();
```

Which might later be translated to SQL like this:

```sql
SELECT [u].[UserId], [u].[Username]
FROM [Users] AS [u]
WHERE (
    SELECT COUNT(*)
    FROM [Purchases] AS [p]
    WHERE [u].[UserId] = [p].[UserId]
) > 5
```

This query is truly not the best I’ve seen due to several reasons. So, what are those reasons:

- Subquery overhead. For each object in the Users table, a subquery is run to check how many purchases were made. If the Users table has a lot of entries, this could be very inefficient.
- No JOIN operations. In this case, the JOIN operation might be more efficient to use, however, the abstraction might not always choose the most optimal path.

How would we write the same query, if it was handwritten?

```sql
SELECT [u].[UserId], [u].[Username]
FROM [Users] AS [u]
JOIN [Purchases] AS [p] ON [u].[UserId] = [p].[UserId]
GROUP BY [u].[UserId], [u].[Username]
HAVING COUNT([p].[PurchaseId]) > 5
```

In this case, we’re using JOIN operation, then using GROUP BY clause to get rid of the possible duplicates and finally checking the count of purchases with HAVING clause.

## 5. Security concerns with RAW SQL

In case you didn’t know, EF lets you execute raw SQL. There are multiple ways to do so, but the most commonly used ones are Migrations and direct execution from DbContext. Both of the methods accept interpolated strings as parameters and there’s a huge flaw in security.

**Migrations**

While executing raw SQL from Migrations, you’ll need to add an empty migration file, where you apply your SQL code later.

`dotnet ef migrations add createstoredprocedure`

```csharp
// Migration code
// ...
migrationBuilder.Sql($"YourSqlQuery ${parameter}");
```

In terms of creating stored procedures with raw SQL from EF there’s nothing wrong. The problem appears when you are calling it from EF. You might think, what’s the worst that can happen?

Well, EF is accepting RAW SQL as an interpolated string, which means your parameters are not being converted to SQLParameter class, because it’s the only way EF supports running RAW SQL.

**DbContext**

So you might say, if EF has a chance of generating bad SQL code, why don’t I use it to run RAW SQL for complex queries? And you could do it, but really carefully. That’s because of the reason I already stated above, EF is not converting your parameters to SQLParameter class, it directly executes all the SQL you give it. The way you do it through DbContext is as follows:

`_context.Users.FromSqlInterpolated($"SELECT FirstName FROM dbo.Users WHERE Id = {id}");`

As you see, your parameter is directly passed to the SELECT statement. It is not good, to put it mildly. You always got to be careful with such tools, if you screw anything up, you might as well drop your whole DB.

**Conclusion**

{{<img src="/images/dapper-meme.webp" align="center" alt="Meme about junior dev looking to Dapper">}}

There will never be a way that’s always good or bad, and you can’t say any of those about EF as well. You have to decide what’s best for your application and how are you going to use it. If you have a small and simple application you might use Entity Framework and won’t even need RAW SQL execution. If you’re building something more complex, you might want to look at Dapper, which gives you the ability to execute RAW SQL and Stored Procedures while converting all parameters to SQLParameter class, guarding your DB from SQL Injection attacks. I might write some article on Dapper as well later, but for now, that’s all.

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read! :)
