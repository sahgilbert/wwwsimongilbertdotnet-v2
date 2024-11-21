+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net MVC", "Asp.Net Web Api", "C#", "Chief Technology Officer", "C#.Net", "Code", "Code Snippet", "Coding", "Comparison", "C Sharp", "CTO", "Dev", "Dot Net", "GIST", "GISTS", "Github", "MVC Core", ".Net Core", ".Net Framework", "Object Oriented Programming", "Programming", "REST", "RESTful", "REST Api", "RESTful Api", "Simon Gilbert", "Software Development", "Web Dev", "Web Development"]
date = 2019-05-09T08:30:00Z
description = ""
draft = false
image = "/images/2019/06/simon-gilbert-dev-cto-blog-27.png"
slug = "unified-mvc-vs-web-api-controllers-aspnetcore"
summary = "Coding both MVC & Web Api combined? Simon Gilbert details unified MVC View & Web Api endpoint controllers in C# ASP.Net Core"
title = "Unified MVC & Web Api Controllers - C# ASP.Net Core"
tags = ["C#.Net", "Asp.Net MVC", "Asp.Net Web Api"]
+++


### ASP.Net MVC 4 Controllers

Recently I worked on a project that was written in **ASP.Net MVC 4**. The tech stack itself was split between **Web Api** and **MVC** itself, in separate projects with different code bases. The lack of unified controllers compared to what **ASP.Net Core** now offers felt like going back in time...

...So given that, let's take a very brief look at unified MVC & Web Api controllers in C# ASP.Net Core!

### Prior To Unification

Before **ASP.Net Core** we essentially had two separate underlying **assemblies** at the core of our web stack (**MVC** vs. **Web Api**), since both implementations were based on the MVC model and therefore required the use of a **Controller** class. **MVC** was based on **System.Web.Mvc** and would produce one type of controller, specifically for the standard ASP.Net MVC design which involved returning **cshtml** views. Whereas **Web Api** was based on **System.Web.Http** and would produce an **ApiController** which allowed the return of **JSON** in a **RESTful** manner.

To add some context, here's a simple example of using both **MVC** and **Web Api** when implementing your code base using **MVC4**. Starting with **MVC**...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-mvc-controller.png" caption="C# ASP.Net MVC 4 - MVC View Controller Action" >}}

...and then **Web Api**, of course separately.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-web-api-controller.png" caption="C# ASP.Net MVC 4 - Web Api Endpoint Controller Action" >}}

There were numerous issues with this design including the copious amounts of configuration required to get simple routing setup, not least the fact that it prevented easy grouping of related actions.

### Enter “ASP.Net Core 2”…

With the advent of **ASP.Net Core** both **MVC** and **Web Api** controller implementations now inherit from the same **Controller** base class, which is part of the **Microsoft.AspNetCore.Mvc** assembly. The unification doesn’t stop here either - The actions have been combined too, along with many other components such as the routing and filters that we are used to taking advantage of.

Here is a glorious **ASP.Net Core 2** unified controller -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-unified-1.png" caption="C# ASP.Net Core - Unified MVC &amp; Web Api Controller" >}}

Enjoy!