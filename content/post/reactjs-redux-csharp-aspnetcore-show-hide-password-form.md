+++
author = "Simon Gilbert"
categories = ["Asp.Net Core Web Api", "Asp.Net MVC", "Asp.Net Web Api", "Bootstrap 4", "C#", "Chief Technology Officer", "Client-Side Validation", "C#.Net", "Code", "Code Snippet", "Coding", "Component", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Full Stack", "Full Stack Web", "Full Stack Web Development", "JavaScript", "MVC Core", ".Net Core", "Object Oriented Programming", "Password", "Passwords", "Programming", "React", "ReactJS", "ReactJS Component", "Redux", "Redux Action", "Redux Reducer", "Redux Store", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Single Page Application", "Software Development", "SPA", "Validation", "Web Dev", "Web Development"]
date = 2020-06-03T14:52:00Z
description = ""
draft = false
image = "/images/2019/07/simon-gilbert-dev-cto-blog-30.png"
slug = "reactjs-redux-csharp-aspnetcore-show-hide-password-form"
summary = "Coding HTML password forms? Simon Gilbert explains how to show an hide password values using ReactJS, Redux, C# ASP.Net Core"
title = "[Full Stack Web] Show & Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core"
tags = ["React.js", "Redux", "Asp.Net Web Api"]
+++


### Password Required

How often do you type a **password** into an **HTML** form? Fairly regularly? ...unless you're using a **password manager** (you should, but we'll cover that another day). So what if you've typed your **password** but you can't be certain that it's correct, and want to see it **before you click submit**?

### Enter "ReactJS"...

I've been using **ReactJS** and **Redux** a fair bit recently, so I figured why not create a fresh **ReactJS**  **component** to encapsulate the logic for showing and hiding a users password in an **input field**?

### Let's Code...

This implementation will be covered as a summary and uses some of the **ReactJS**  **components** that I coded and [demonstrated on previous blogs](/reactjs-redux-csharp-aspnetcore-client-side-validation/). Let's begin with a basic data model written in C# on the server-side.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-1.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Next we need a basic C# ASP.Net Web Api controller and action, to allow our html form to postback to.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-2.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Now let's code the **client-side implementation**, specifically our dedicated component for our password field and it's required functionality, beginning with some imports of the previously written input field components that I mentioned earlier.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-3.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Next is our constructor.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-4.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Now we need to code a function to toggle the state of our input field.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-5.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Now we need to bind this function inside our components constructor.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-6.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Following this let's code our component's render function, specifically making use of the additional components which we imported previously. You'll note that for this implementation we're making use of **Bootstrap 4's** input-group functionality to bolt a button onto the end of our input field for showing and hiding the password.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-7.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Finally, let's make use of the component inside our HTML form.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-8.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Now to run our **full stack application** and test our newly coded component.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-9.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

As you can see, our password input field looks slightly different to the email address input field, since it makes additional use of the input-group appending functionality.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-10.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Now that we've typed a password which is hidden as normal, let's see what happens when we click the show/hide password button.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-11.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

BOOM! There it is, so now let's post our form back to the server.

{{< figure src="/images/2019/07/simon-gilbert-dev-cto-reactjs-password-12.png" caption="Show &amp; Hide Password Form Submit - ReactJS + Redux + C# ASP.Net Core" >}}

Enjoy!