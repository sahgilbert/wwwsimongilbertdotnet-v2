+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net Web Api", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Github", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Object Oriented Programming", "Programming", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Swashbuckle", "Swagger", "Documentation", "Doc", "Docs", "RESTful Documentation", "RESTful Api Documentation", "RESTful Api Docs"]
date = 2019-04-18T10:38:00Z
description = ""
draft = false
image = "/images/2019/05/simon-gilbert-dev-cto-blog-22.png"
slug = "aspnetcore-web-api-swashbuckle-swagger"
summary = "Need to document your RESTful Api?\n Simon Gilbert explains how to document endpoints using Swashbuckle Swagger for C# ASP.Net Core Web Api"
title = "Documenting your RESTful C# ASP.Net Core Web Api using Swashbuckle & Swagger"
tags = ["C#.Net", "ASP.Net Web Api", "Swashbuckle", "Swagger", "Documentation"]
+++


### RESTful Api Endpoints

How often have you been in a situation where you're having to **code** against an undocumented **API**? Perhaps your team is building an **API** and publishing it to another group of developers. Understanding the **API** endpoints without the correct documentation can be a challenge...

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-1.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

### Enter "Swagger"...

**Swagger** enables you o generate interactive documentation as a readable representation of a **RESTful API**.

In order to generate **Swagger** documents for our **C# ASP.Net Core Web Api** application, we need to utilise **Swashbuckle** - An open source project.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-2.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

### Let's Code...

I've implemented a basic **C# ASP.Net Core Web Api** solution with 2 public endpoints for us to document.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-3.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Now to pull in the required **Swashbuckle** packages from **Nuget**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-4.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Now to add our configuration into the **appsettings** file.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-8.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Head to your **Startup.cs** file and let's configure our **Swagger** middleware. Our previously added configuration details can now be made use of.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-5.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Now to make use of the variables and configure our Swagger service.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-6.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

We can now configure our Swagger endpoint.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-7.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Finally let's head to the endpoint URL -

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-10.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

...and here's the result!

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-swagger-9.png" caption="C# ASP.Net Core Web Api - Swashbuckle Swagger" >}}

Enjoy!