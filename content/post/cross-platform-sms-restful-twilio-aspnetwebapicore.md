+++
author = "Simon Gilbert"
categories = ["Asp.Net Web Api", "Asp.Net Core Web Api", "C#", "Chief Technology Officer", "C#.Net", "C Sharp", "CTO", "Dot Net", "Microsoft Visual Studio", ".Net Core", "Object Oriented Programming", "Simon Gilbert", "Twilio", "Twilio SMS", "SMS", "RESTful", "REST", "REST Api", "RESTful Api", "Cross-Platform", "Code", "Coding", "Computer Science", "Dev", "Nuget", "Programming", "Software Development", "Web Dev", "Web Development"]
date = 2019-03-12T08:17:41Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-eight-2.png"
slug = "cross-platform-sms-restful-twilio-aspnetwebapicore"
summary = "Looking to send SMS to your users?\n Simon Gilbert discusses how to construct a cross-platform RESTful API for sending SMS messages using the Twilio platform and C# ASP.Net Core Web Api in Visual Studio for Mac."
title = "Cross-Platform SMS RESTful Api using Twilio - C# ASP.Net Core Web Api"
tags = ["C#.Net", "Asp.Net Web Api", "RESTful Api", "Twilio", "SMS"]
+++


### Short Message Service (SMS)

Originating from the radio telegraphy used in pagers, modern day SMS still adheres to the standards of the **Global System for Mobile Communications (GSM)**.

...The part that always perplexes people is the mechanism which allows you to hook your intermediary language into a telecommunications protocol.

### Enter... ”Twilio”

> “Take your ideas from localhost to the world”

...And that’s exactly what we’re going to do, using the **Twilio SMS Api**...but in this implementation we’re going to code on the assumption of needing cross-platform scalability in future...

### Distributed System Integration

When it comes to scalability, the design of a platform is correlated to the separation of features which are likely to undergo the largest amount of throughput. The beauty of this is that you can handle the scale of a particular features server in a different manner to the less in demand parts of the platform.

...By combining our Twilio integration with a **RESTful** service wrapper written in **ASP.Net Core Web Api** (Microsoft’s cross-platform framework), we can separate our concerns and allow a layer of business logic to sit over the top of our SMS protocol (whilst ultimately not binding ourselves to a specific operating system).

### Code Time

Let’s start with our underlying data model. I’ve gone with some fairly obvious fields here to allow for flexibility in our Api wrapper and the tone we want to convey to each receiver of an SMS message. Remember, flexibility is key to help preempt future unnecessary rewrites.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-sms-message-model.png" caption="ASP.Net Core Web Api SMS Message Model" >}}

Before we put the service together, we’ll need a generic response model for returning a different response from the endpoint, dependent on what happened when an attempt to send an SMS occurred.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-generic-api-response-model.png" caption="ASP.Net Core Web Api Generic API Response Model" >}}

Pull in a copy of the Twilio.AspNet.Core Nuget package to your Web Api project and create an interface based class with an async method for sending SMS via Twilio.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-twilio-sms-service.png" caption="ASP.Net Core Web Api Twilio SMS Service" >}}

Great. Now that we’ve got that sorted, it’s time to add some validation to our business layer. Remember the **System.ComponentModel.DataAnnotations** namespace and the ever faithful **[Required]** attribute? ...Yes, that isn’t the most appropriate solution unless we want to bolt out model validation to our underlying contract.

Head over to **Nuget** again and pull in a copy of the **FluentValidation** package. We’re going to code a validation service layer that adheres to the single object responsibility principle...thus keeping things, separate? As a side note, if your class or components responsibility includes the word “and”, you’re breaking the principle...

I'm specifically coding this under the assumption that it’s UK to UK SMS messaging only...so, a few key things here - UK mobile phone numbers are 13 characters including the country code. Also, Twilio won’t send SMS to a number without a country code, and the other fields are required for constructing an accurate message body.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-fluentvalidation.png" caption="ASP.Net Core Web Api SMS FluentValidation" >}}

Ok now that the key underlying aspects of the implementation have been coded, it’s time to plug them into our controller and expose the service as a cross-platform RESTful Api component. I’ll refrain from explaining the obvious here and skip to covering the attributes on the controllers action.

When a request contains input parameters, the number of potential return types increases. Following the release of ASP.NET Core 2.1, the return type **ActionResult<T>** was introduced for Web API controller actions. This enables us to return a type deriving from ActionResult, or, return a specific type.

Given this, we can make use of the **[ProductResponseType]** attribute. This indicates the known types and HTTP status codes to be returned by the action itself. It’s very clean!

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-aspnetcorewebapi-controller-action.png" caption="ASP.Net Core Web Api SMS Controller Action" >}}

### Let’s hook into Twilio Now...

Head to their website and sign up for an account. There are three things we need next -

1) A mobile phone number to send SMS from.

2) An account SID.

3) An authentication token.

Twilio will allow you to generate a free trial UK mobile phone number for sending SMS from...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-twilio-free-trial-number.png" caption="Twilio SMS Platform Free Trial Mobile Phone Number" >}}

...as well as obtaining the required connection strings for hooking into the telecommunications protocol.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-twilio-dashboard.png" caption="Twilio SMS Platform Dashboard Connection Strings" >}}

Next, let’s embed the SID and auth token from Twilio into our API appsettings file...

{{< figure src="/images/2019/03/simon-gilbery-cto-tech-blog-post-twilio-connecion-strings-appsettings-1.png" caption="ASP.Net Core Web Api appsettings Twilio Connection Strings" >}}

...and now configure it at startup -

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-aspnetcorewebapi-twilio-init-configure.png" caption="ASP.Net Core Web Api Twilio Init Startup Configure" >}}

### My Good Friend, Postman

So our Twilio SMS REST Api wrapper is ready to test. Let’s launch postman, and submit an HTTP Post to our endpoint. The first attempt will involve incorrect data to provoke a reaction from our validation service.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-aspnetcorewebapi-postman-bad-request.png" >}}

As you can see, the response is indicative of the occurring validation errors which are embedded in our generic response. Next let’s try a valid **HTTP POST** submission.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-aspnetcorewebapi-postman-success.png" >}}

Note the **HTTP 200** response...were specifically returning the Twilio platform response from our API endpoint, in scenarios where no validation errors occur and the message is submitted.

### And here’s the result...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-aspnetcorewebapi-twilio-sms-received.png" caption="Twilio SMS Received" >}}

Congratulations, you're now running a cross-platform API for sending SMS via code using Twilio!

Enjoy!