+++
author = "Simon Gilbert"
date = 2019-12-23T22:11:00Z
description = ""
draft = false
image = "/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc.png"
slug = "automapper-csharp-aspnetcore-mvc-3-0"
summary = "Need to map between view models and database models? Simon Gilbert explains a performant way of doing this using AutoMapper in C# Asp.Net Core MVC 3.0"
title = "Using AutoMapper with ASP.Net Core MVC 3.0"
tags = ["C#.Net", "Asp.Net MVC", "AutoMapper"]
+++


### Models vs. View Models

Occasionally you will require the ability to separate a model for a view, from the equivalent model for a database. But what happens when you need to transfer the view model data to the database model object?

### Enter "AutoMapper"

AutoMapper is an object-object mapper, and makes this mapping task fairly straight-forward (albeit, following some configuration).

{{< figure src="/images/2019/12/Simon_Gilbert_Blog_AutoMapper_Logo.png" caption="AutoMapper" >}}

### Implementation Steps

* Install a copy of "**AutoMapper.Extensions.Microsoft.DependencyInjection"** from **Nuget**.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_1.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Create a view model.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_2.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Create a model.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_3.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Create an AutoMapper **"Profile"** (class which inherits from the Profile class in AutoMapper), for defining the map between the view model and the model.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_4.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Configure Services in Startup.cs to add **AutoMapper** to the project.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_5.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Create a service for executing the model mapping.

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_6.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

* Configure Services in Startup.cs for your mapping service (as a transient service).

{{< figure src="/images/2019/12/Simon_Gilbert_AutoMapper_Aspnetcoremvc_7.png" caption="AutoMapper with ASP.Net Core MVC 3.0" >}}

Enjoy!