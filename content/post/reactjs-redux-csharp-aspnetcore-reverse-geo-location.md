+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net MVC", "Asp.Net Web Api", "Bootstrap", "Bootstrap 4", "Bootstrap Framework", "Bootstrap Framework 4", "C#", "Chief Technology Officer", "Client-Side Validation", "C#.Net", "Code", "Code Snippet", "Coding", "Component", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Full Stack", "Full Stack Web", "Full Stack Web Development", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Object Oriented Programming", "Programming", "React", "ReactJS", "ReactJS Component", "Redux", "Redux Action", "Redux Reducer", "Redux Store", "Simon Gilbert", "Single Page Application", "Software Development", "SPA", "Validation", "Validation Rules", "Web Dev", "Web Development"]
date = 2020-05-23T12:07:00Z
description = ""
draft = false
image = "/images/2019/07/simon-gilbert-dev-cto-blog-29-1.png"
slug = "reactjs-redux-csharp-aspnetcore-reverse-geo-location"
summary = "Reverse engineering geo location? Simon Gilbert explains this via postcodes.io using ReactJS, Redux, C# ASP.Net Core"
title = "[Full Stack Web] Reverse Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core"
tags = ["React.js", "Redux", "Asp.Net Web Api"]
+++


### Geo Location

I blogged recently about [tracking a users geo location](/iphone-geo-location-csharp-xamarin-ios/) from their **iPhone**, but what if all you have is a postcode, yet still require coordinate data?

### Enter "www.postcodes.io"...

I discovered [this third party api](https://postcodes.io) not long ago and it's been very useful. Typically the most common downside with these api's is that [they're rate limited](/rate-limit-action-filter-attribute-csharp-aspnetcore/), so today we're going to code a full stack single page app (**SPA**)...with a **built in memory cache**, thus reducing how often we're hitting the endpoint for the same postcode.

### Prerequisites

You'll need to follow [the steps I blogged about here](/full-stack-spa-reactjs-redux-bootstrap4-aspnetcore/) to configure your single page app (**SPA**) setup to run with **ReactJS** + **Redux** + **C# ASP.Net Core**.

### Let's Code...

Firstly we need a data model to interact with www.postcodes.io -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-geo-1.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Next, a web client service to ping the endpoint.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-geo-2.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Now we need a dedicated service to handle the requests we pass in, and the output we receive back.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-geo-3.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Now to setup our API controller and memory cache -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-geo-4.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Followed by our controller action method.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-geo-5.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Now to code our client-side in **ReactJS**. Let's start with some imports (I'll be using the **InputField**, **Button** and **Table** components that I've written and published recently in other blog posts). You'll note that I've also added a massive **regex** to be used when validating the postcodes.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-6.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Next we'll need a few properties to help with the field validation.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-7.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Now for a function to handle the user's input.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-8.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Following this comes the field validation functionality.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-9.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

At this point we can render the form, using the **components** I mentioned previously.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-10.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Now to plug our **ReactJS** client-side code into our **C# ASP.Net Core** server-side code using **Redux**.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-11.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Let's run our application and see what happens.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-12.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Our validation has kicked in and requires a valid postcode before we can submit the form.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-13.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Once we enter a valid postcode, our submit button becomes enabled and the postcode regex validation has passed.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-geo-14.png" caption="Revere Geo Location Lookup - ReactJS + Redux + C# ASP.Net Core" >}}

Finally, we receive the relevant geo location coordinates for the postcode we submitted, indicating the latitude and longitude.

Enjoy!