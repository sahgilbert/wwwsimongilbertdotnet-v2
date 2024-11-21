+++
author = "Simon Gilbert"
categories = ["Architecture", "Asp.Net Core Web Api", "Asp.Net Web Api", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Github", "Microsoft Visual Studio", ".Net Core", "Nuget", "Object Oriented Programming", "Performance", "Programming", "REST", "REST Api", "RESTful", "RESTful Api", "RESTful Api Docs", "RESTful Api Documentation", "RESTful Documentation", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Full Stack", "Single Page Application", "SPA", "React", "ReactJS", "Redux", "Bootstrap", "Bootstrap Framework", "Twitter Bootstrap", "Twitter Bootstrap Framework", "Bootstrap 4", "Bootstrap Framework 4", "Twitter Bootstrap Framework 4"]
date = 2020-04-06T07:59:00Z
description = ""
draft = false
image = "/images/2019/05/simon-gilbert-dev-cto-blog-26.png"
slug = "full-stack-spa-reactjs-redux-bootstrap4-aspnetcore"
summary = "Need to code full stack? Simon Gilbert explains how to setup a SPA using ReactJS, Redux, C# ASP.Net Core & Bootstrap 4"
title = "[Full Stack] Single Page App (SPA) - ReactJs + Redux + Bootstrap 4 + ASP.Net Core"
tags = ["React.js", "Redux", "Asp.Net Web Api", "Bootstrap", "SPA"]
+++


### Coding On "Performance" Trend

From experience, a key requirement from stakeholders is always **_"performance"_**, and quite rightly so. Over the past 5 years there has been a large industry shift towards making use of **Javascript libraries** for creating powerful and beautiful user interfaces, but what if you need to **maintain state** and go full stack with **ASP.Net Core** on the server-side?

Today, we're going to look at getting setup with a **full stack single page application (SPA) implementation** of **ReactJS + Redux + Bootstrap 4 + ASP.Net Core**.



{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-1.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Enter "ReactJS"...

**ReactJS** was created by **Jordan Walke**, a software engineer at **Facebook**.

> "**ReactJS** is a JavaScript library for building user interfaces. It is maintained by **Facebook** and a community of individual developers and companies."

**ReactJS** makes heavy use of the Javascript syntax extension **JSX** (Javascript XML), which essentially makes **Javascript** look like **Html** written in a string, and is generally what components in **React** are written in, although they can be written in pure **Javascript** also.

**ReactJS** is component based, so only **each component on a page** needs to be updated. Remember **AJAX**? ...imagine that with a more structured design and you're halfway there.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-2.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Enter "Redux"...

**Redux** is a "predictable state container" - It shares state across all components within an application. Redux has a store, which holds state for the entire application and the data is supplied as properties (this.props).

> "**Redux 4** has a much smaller footprint because they removed the lodash dependency (package management)."

Redux has three core components - **Actions, Reducers, Store**.

**Actions** - These are the payload of information that is sent from the application to the store. They are merely plain **Javascript** objects with the type defined as a string, and are created by an "action creator".

**Reducers** - These specify how the state changes in response to an action. Reducers take the Action payload, and apply it to the store. The state is immutable, so a reducer returns a new state with whatever needs to be added, updated or removed.

**Store** - This holds the state and allows access to the state. The store allows the state to be updated, whilst also registering listeners to keep an eye on the state when it changes in different modules...

...essentially, Redux is quite powerful and extremely useful!

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-3.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Enter "Bootstrap 4"...

I'm sure you know the **Bootstrap** story already - A **mobile-responsive framework** created by a dedicated team at **Twitter** originally in 2011 in order to maintain consistent implementations during **front-end** development. **Bootstrap 3** was written in **LESS**, however **Bootstrap 4** is written in **SASS** with the grid system based on **REMs**. It's also worth noting that **BS4** has a 64% smaller footprint than **BS3** too. **Bootstrap 4** is what we'll be using today!



{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-4.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Enter "ASP.Net Core"...

Finally, our server-side framework will be running on ASP.Net Core. I'll refrain from explaining this here, other than stating that it's cross-platform and totally awesome!

### Let's Code...

First of all, you need to be running the latest version of **Node** on your machine.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-5.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Next we're going to use the relevant **reactredux template** to create our project, which for this example I've called "full-stack-app" (and this name must be entirely lowercase, but we can change it later).

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-6.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

We then need to CD into that directory to see the template files that have been created by our previous terminal statement.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-7-1.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Once in that directory, open the **.csproj** file in **Visual Studio** (this generates a solution file). Save all, and close Visual Studio. You can then rename the **solution** and your **.csproj** file.

Following this, open the **solution** in **Visual Studio**, clean, rebuild. Then find and replace all of the original name _(full-stack-app)_, to the new name (in my case **SimonGilbert.Blog**). _When doing this, be sure to include both **full-stack-app** and the same name but with underscores i.e. **full_stack_app**._

Ok, now to upgrade some key packages. First, CD into the **ClientApp** subdirectory (this folder houses all the UI source code and packages).

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-8.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Let's update the **JQuery** version.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-9.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Now the **prop-types** package also.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-10.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Followed by an upgrade to the latest version of **React**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-11.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

...including the **React DOM**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-12.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

And then **Reactstrap** (we'll need this for **Bootstrap** usage later).

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-13.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Next we need to upgrade **Redux** to the latest version.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-14.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Followed by **Redux Thunk**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-15.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

And finally, **NPM audit fix**.....to fix any issues that have occurred along the way (although some of the dependency errors can indeed be ignored).

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-16.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Reviewing Our Changes

If you now open the **solution** and navigate to the **packages.json** file, you should see something similar to this -

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-17.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

### Running Our Application

Run the application as normal from Visual Studio, and you should see something similar to the following -

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-18.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

...Result! Now head to the **"Fetch data"** tab which will load a series of weather forecast data from the **server-side**. If you now navigate o the controllers folder in Visual Studio, you'll see the **ASP.Net Core Web Api** code endpoint that **Redux** is interacting with via **ReactJS**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-reactjs-redux-aspnetcore-19.png" caption="Full Stack Single Page App (SPA) Setup - ReactJs + Redux + Bootstrap 4 + ASP.Net Core" >}}

Enjoy!