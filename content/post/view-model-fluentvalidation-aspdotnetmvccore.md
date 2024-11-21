+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "C#", "Chief Technology Officer", "C#.Net", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dot Net", "FluentValidation", "Microsoft Visual Studio", "MVC Core", ".Net Core", ".Net Framework", "Object Oriented Programming", "Nuget", "Simon Gilbert", "Validation", "Validation Rules", "Code", "Coding", "Dev", "Programming", "Software Development", "Web Dev", "Web Development"]
date = 2019-03-23T01:00:00Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-eleven.png"
slug = "view-model-fluentvalidation-aspdotnetmvccore"
summary = "Looking to validate an HTML view at post back? Simon Gilbert explains the steps required to bind your validation rules to an HTML view, using the FluentValidation library and C# ASP.Net MVC Core."
title = "Binding Fluent Validation to an HTML View - C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Validation", "Fluent Validation"]
+++


### Strongly-Typed Data Model Validation

[Previously I discussed](/validation-rules-fluentvalidation-csharp-dotnetcore/) using the [FluentValidation](https://fluentvalidation.net/) library to implement strongly-typed data model validation in C# .Net Core. Today we're going to look at injecting that validation to an HTML view in C# ASP.Net MVC Core, making use of the standard **ModelState** syntax in MVC.

### Nuget Packages

You’ll need to pull in a copy of **FluentValidation** and **FluentValidation.AspNetCore** from Nuget.

### Let’s Code...

First of all we need a basic view model with some fields to validate.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-data-model-1.png" caption="C# ASP.Net MVC Core - Fluent Validation Data Model" >}}

### Abstract Validator Inheritance

Now that we have a data model, we can create a validation class which inherits from AbstractValidator. In this case I’ve separated the FirstName field from the technical data about our new user for cleanliness.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-view-model-validator.png" caption="C# ASP.Net MVC Core - Fluent Validation Abstract Validator" >}}

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-inherit-abstract-validator.png" caption="C# ASP.Net MVC Core - Fluent Validation View Model Validator" >}}

### Startup Configuration

Next we need to inject the validator class into our platform at startup.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-configure-services.png" caption="C# ASP.Net MVC Core - Fluent Validation Configure Services" >}}

### HTML View

Let’s put together a simple form with a post back for our data model.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-html-aspdotnetmvccore.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML View" >}}

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-aspdotnetmvccore-1.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML" >}}

Now let's submit a blank version of our page data to the controller.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-aspdotnetmvccore-2.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML Errors" >}}

As you can see, our fluent validation has kicked in with very little else required in terms of implementation. Let’s amend the data slightly and see how our validation errors change.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-aspdotnetmvccore-3.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML Different Errors" >}}

Finally, let’s submit a fully valid post back

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-aspdotnetmvccore-4.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML Valid" >}}

...and see how our fluent validation responds?

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-fluentvalidation-aspdotnetmvccore-5.png" caption="C# ASP.Net MVC Core - Fluent Validation HTML Successful Validation" >}}

...**Success**! As mentioned before, **FluentValidation** provides a much cleaner method of carrying out strongly typed data model validation than the traditional **DataAnnotations** namespace.

Enjoy!