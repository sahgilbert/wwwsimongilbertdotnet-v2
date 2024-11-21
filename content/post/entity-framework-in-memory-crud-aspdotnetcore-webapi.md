+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net Web Api", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dot Net", "Github", ".Net Core", "Object Oriented Programming", "Programming", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Entity-Framework Core", "Entity Framework Core", "O/RM", "In-Memory Database", "In-Memory", "EFCore", "EF-Core"]
date = 2019-04-11T06:10:00Z
description = ""
draft = false
image = "/images/2019/04/simon-gilbert-dev-cto-blog-20.png"
slug = "entity-framework-in-memory-crud-aspdotnetcore-webapi"
summary = "Need an in-memory database? Simon Gilbert explains Microsoft Entity Framework Core's' in-memory O/RM database as a CRUD layer in ASP.Net Core Web Api"
title = "RESTful CRUD Api using Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api"
tags = ["C#.Net", "Asp.Net Web Api", "RESTFul Api", "Entity Framework"]
+++


### Use Case

I recently needed to implement a **CRUD** layer in a **RESTful Api**, however the decision regarding which **database context** to use to store our data hadn't quite happened. Given this, I decided to build an **abstract implementation** which would allow us to **_"plug-and-play"_** our chosen database framework into the **Api** at a later date.

Rather than going down the route of spinning up a **cloud database** to test out the **abstraction**, I decided to use an i**n-memory database context**...

### Enter "Microsoft's Entity Framework Core"...

> "Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology. EF Core serves as an **object-relational mapper (O/RM)**, enabling .NET developers to work with a database using **.NET objects**, and eliminating the need for most of the data-access code they usually need to write."

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-1.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}



If you've ever had the luxury of using the **Entity Framework** you're already well aware of how capable it is and how awesome **O/RM** is, not to mention how **clean** the implementation is to work with. Fortunately they now support **.Net Core**, so let's give it a whirl and build a **RESTful CRUD Api** using the **Entity Framework In-Memory Database**!

### Let's Code...

Today's example is based on storing a series of blog posts via a **RESTful CRUD Api**. First, create a fresh **ASP.Net MVC Core** project and then pull in a copy of the relevant package from **Nuget**.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-2.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Our abstraction will work as follows: **Controller > Service > Repository > Database Context**

Let's start by defining a data model to use -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-3.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Next we need to code our database context layer by defining our database table for our blog post entities, and setting our primary key.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-4.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Now to inject our database context into our repository via ServiceScope.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-5.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

We can now add a basic "create" method to our repository.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-6.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Next we inject our repository into our service layer, along with a "create" method which binds to the equivalent repository method.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-7.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Now we can inject our service into our controller.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-8.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

...Followed by our controller action for our "create" process flow, along with some simple validation via **FluentValidation**.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-9.png" caption="Entity Framework Core's In-Memory (O/RM) Database - C# ASP.Net Core Web Api" >}}

Finally we can configure our **dependency injection** within our **Startup.cs file**.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-10.png" >}}



### Testing Our Implementation

Let's fire up Postman and submit data to our Api endpoints...

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-blog-orm-11.png" >}}

...Result! 

Enjoy!