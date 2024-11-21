+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net MVC", "Asp.Net Web Api", "Bootstrap", "Bootstrap 4", "Bootstrap Framework", "Bootstrap Framework 4", "C#", "Chief Technology Officer", "C#.Net", "Code", "Code Snippet", "Coding", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Full Stack", ".Net Core", "Object Oriented Programming", "Programming", "React", "ReactJS", "Redux", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Single Page Application", "Software Development", "SPA", "Twitter Bootstrap", "Twitter Bootstrap Framework", "Twitter Bootstrap Framework 4", "Validation", "Validation Rules", "Web Dev", "Web Development", "Full Stack Web", "Full Stack Web Development", "JavaScript", "Client-Side Validation", "Component", "ReactJS Component", "Redux Action", "Redux Reducer", "Redux Store"]
date = 2020-05-14T14:38:00Z
description = ""
draft = false
image = "/images/2019/06/simon-gilbert-dev-cto-blog-28.png"
slug = "reactjs-redux-csharp-aspnetcore-client-side-validation"
summary = "Need to validate data integrity? Simon Gilbert explains SPA client-side validation using ReactJS, Redux, C# ASP.Net Core"
title = "[Full Stack Web] Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core"
tags = ["React.js", "Redux", "Asp.Net Web Api", "Validation"]
+++


### Early Validation

When it comes to users entering data, it's important to always **validate the integrity** before storing the information. The earlier you can do that the better, so why validate **server-side** when you can validate **client-side** before submitting the postback?

### Prerequisites

You'll need to follow [the steps I blogged about here](/full-stack-spa-reactjs-redux-bootstrap4-aspnetcore/) to configure your single page app (**SPA**) setup to run with **ReactJS** + **Redux** + **C# ASP.Net Core**.

### Let's Code....

Assuming you've setup your local repository as per the [aforementioned blog](/full-stack-spa-reactjs-redux-bootstrap4-aspnetcore/), the first thing to code is a **component to hold our dynamic label**. Everything in this example will be based on **Bootstrap 4**. Validation icons are cool, so let's add one of those which can be dynamically change as and when necessary.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-1.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Next is the **label component** itself, which will need to import our validation icon to be used as and when necessary.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-2.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Now that our label component is coded, let's add a further component to hold our **validation error messages**.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-3.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Next we need to construct an **input field component** which imports our label and field error components.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-4.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Now that our input field component is constructed, we need a **button component** which will remain disabled until all input components are valid.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-5.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

We now need a data model for our server-side code, written in **C# .Net Core**.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-5b.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Next, some more server-side code (our api controller and a static list to store the data in **C# ASP.Net Core**).

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-6.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Now for the part where **Redux** comes in to facilitate our connection between the **client-side** and the **server-side**. We'll expose a _"submitForm"_ function within our action creators, to dispatch and receive our request and response.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-7.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

...accompanied by some simple reducer statements.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-8.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Next we need to code the client-side validation, starting with some constants in our SignUpForm component -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-9.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Following this, we'll need a constructor and some properties to manipulate when the user interacts with the form.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-10.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Now we can render our form component, using the input field component we coded at the beginning.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-11.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

You'll note that each input field has a function attached to its onChange method, specifically for handling user input -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-12.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Now to validate the inputs with a glorious switch statement, depending on the field name.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-13.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Let's run our build and see what we've got -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-14.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

...and to test the validation as we type in our name -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-15.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Followed by our email address, which validates differently based on our switch statement.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-16.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Here we can clearly see the valid fields vs. the invalid fields -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-17.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Eventually our end result enables our form submission button -

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-18.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Clicking submit now shows us our received response from our **C# ASP.Net Core** api.

{{< figure src="/images/2019/06/simon-gilbert-dev-cto-reactjs-valid-19.png" caption="Client-Side Validation Postback - ReactJS + Redux + C# ASP.Net Core" >}}

Enjoy!