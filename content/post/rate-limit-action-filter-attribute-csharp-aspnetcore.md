+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net MVC", "Asp.Net Web Api", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Cyber Attack", "Cyber Security", "DDOS", "Dev", "Hacking", "In-Memory", "In-Memory Database", "Memory Cache", ".Net Core", ".Net Framework", "Object Oriented Programming", "Programming", "Protection", "REST", "REST Api", "RESTful", "RESTful Api", "Secure", "Security", "Simon Gilbert", "Software Development", "Software Patterns", "Throughput", "Web Dev", "Web Development"]
date = 2019-05-02T06:37:00Z
description = ""
draft = false
image = "/images/2019/05/simon-gilbert-dev-cto-blog-25.png"
slug = "rate-limit-action-filter-attribute-csharp-aspnetcore"
summary = "Need to rate limit your Api? Simon Gilbert explains how to use action filter attributes in C# ASP.Net Core Web Api"
title = "Rate Limiting RESTful Api Requests using Action Filter Attributes - C# ASP.Net Core Web Api"
tags = ["C#.Net", "Asp.Net Web Api", "RESTful Api", "Performance"]
+++


> "In computer networks, rate limiting is used to control the rate of traffic sent or received by a network interface controller and is used to prevent DDoS attacks."

...We're not going to be looking at preventing DDoS attacks today, but we are going to look at a simple method for limiting the number of times your users can request a particular endpoint within your C# ASP.Net Core Web Api.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-rate-limit-1.png" caption="Action Filter Attribute Rate Limiting - C# ASP.Net Core Web Api" >}}



### Enter "ASP.Net Core Action Filter Attributes"...

The **C#**  **ASP.Net MVC**  **Core** framework supports four different types of filters:

1. Authorization filters – Implements the `IAuthorizationFilter` attribute.
2. Action filters – Implements the `IActionFilter` attribute.
3. Result filters – Implements the `IResultFilter` attribute.
4. Exception filters – Implements the `IExceptionFilter` attribute.

Today we're going to look at using an action filter, which is an attribute. You can apply most action filters to either an individual controller action or an entire controller.

> "Action filters are used to implement the logic that get executed before or after a controller action executes."



### Let's Code...

To begin with, we need a class that inherits from ActionFilterAttribute.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-rate-limit-2.png" caption="Action Filter Attribute Rate Limiting - C# ASP.Net Core Web Api" >}}

Our **action filter attribute** contains a few properties - A name for uniqueness, an integer to store the **number of seconds** we're basing our **rate limiting** on, and a cache for managing our rate limiting.

Next, we need to override the virtual **OnActionExecuting** method from our inherited class. Within this method we are doing the following -

1) Obtaining the users **ip address**.

2) Storing the **ip address** in our memory cache, with a timeout based on the **number of seconds** we have assigned to our rate limiting action filter attribute.

3) Returning an **error message** and a relevant status code **(HTTP 429)**, in the event that the user hits our rate limit for the Api.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-rate-limit-3.png" caption="Action Filter Attribute Rate Limiting - C# ASP.Net Core Web Api" >}}



Now to apply our action filter attribute to our desired controller action. I've added a simple **Api endpoint** for this example, and applied the **attribute** to the method, stating that we want to rate limit to **1 request, every 5 seconds**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-rate-limit-4.png" caption="Action Filter Attribute Rate Limiting - C# ASP.Net Core Web Api" >}}

Let's now submit multiple requests to our endpoint and see what happens -

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-rate-limit-5.png" caption="Action Filter Attribute Rate Limiting - C# ASP.Net Core Web Api" >}}

...As expected, our action filter attribute returned an error message, along with an **HTTP 429** "Too Many Requests" status code.

Enjoy!