+++
author = "Simon Gilbert"
categories = ["Appveyor", "Asp.Net MVC", "Azure Web App Service", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Continuous Delivery", "Continuous Deployment", "Continuous Integration", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Microsoft Azure", "MVC Core", ".Net Core", "Object Oriented Programming", "Programming", "Simon Gilbert", "Software Development", "Testing", "Tests", "Unit Testing", "Unit Tests", "Web Dev", "Web Development", "YAML", "XUnit"]
date = 2020-02-05T06:09:00Z
description = ""
draft = false
image = "/images/2019/04/simon-gilbert-dev-cto-blog-21.png"
slug = "xunit-ci-azure-appveyor-aspdotnetmvccore"
summary = "Continuously deploying via Appveyor CI? Simon Gilbert explains only deploying an Azure Web App Service if all your xUnit tests pass in ASP.Net MVC Core"
title = "xUnit \"Test Driven\" Continuous Integration to Microsoft Azure Web App Services using Appveyor - C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Azure Web App Service", "XUnit", "CI/CD"]
+++


### Continuous Deployment FTW?

You've decided to use a **continuous integration server (CI)** as part of your **DevOps** lifecycle. You're planning to approach things on a continuous deployment basis, specifically targeting a **Microsoft Azure Web App Service** server. Continuous deployment is indeed fantastic...but what if deployments are made when your unit tests are failing?

### Enter "Appveyor" & "xUnit"...

I've covered Appveyor [previously](/continuous-integration-azure-service-appveyor-aspdotnetmvccore/) when discussing the downside of relying on using a single **continuous integration framework** for delivery. When it comes to testing, I'm sure you're already aware of **xUnit** - An awesome unit testing framework and indeed my favourite to use of them all. I've mentioned **xUnit** previously both [**here**](/unit-testing-xunit-csharp-dotnetcore/) and [**also here**](/aspnetmvccore-mocking-testing-moq-xunit/).

### Let's Code...

Rather than explaining continuous integration or **[YAML](https://www.appveyor.com/docs/appveyor-yml/)** in detail, let's dive straight in to the bit we're most concerned about - **unit testing pre-deployment!**

_I'm working on the assumption that you're running **ASP.Net MVC Core** and have already setup a **Microsoft Azure Web App Service**, along with an **Appveyor** account that is bound to your Github repository and set to respond to all Git commits that you make to the master branch._

Head to **Visual Studio** and add a fresh **xUnit "Test Project" class library** -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-1.png" >}}



Write some **unit tests** that are relevant to your projects features -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-2.png" >}}



Your **Visual Studio** solution should now look something like this -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-3.png" >}}





Immediately after our "build configuration", but before our "artifacts configuration", we need to add a simple **"test configuration"** which includes some additional **YAML** commands -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-4a.png" >}}

...Now to explain what the additional **YAML** commands _(dotnet build and dotnet test)_ do. You'll note that the path we are executing **"dotnet build"** against is the exact path of our **xUnit test project** within our **Visual Studio** solution (I've purposely stated the path directly for clarity). This command builds a project and all of its dependencies. It's also worth nothing that **"dotnet restore"** is run implicitly when you execute **"dotnet build"** (as of **.Net Core 2.0**). "dotnet test" is the test driver that is used to execute all unit tests. So what we're asking **Appveyor** to do here is "build our **xUnit** test project, and then execute the unit tests".

Both our unit tests are set to pass, and pass locally, so lets commit the change to Github and watch Appveyor work its magic!

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-5.png" >}}

A **successful test execution** occurs, with all unit tests passing.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-6.png" >}}

Deployment complete, build success. Due to all of our unit tests passing, **Appveyor** packages up our **ASP.Net MVC Core** website into an artifact and deploys it to our **Microsoft Azure Web App Service**.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-7.png" >}}

If we view the **"tests"** tab in **Appveyor**, we can see our 2 successfully passing unit tests -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-8.png" >}}

We can also view our website package on the **"artifacts"** tab.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-9.png" >}}

...._Now let's break one of our unit tests **by changing our expected result**_ and see how **Appveyor** responds -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-10.png" >}}



As you can see, at the point where **Appveyor** executed the **"dotnet test"** command, the broken unit test fails as expected, and the build ceases to deploy to our **Microsoft Azure Web App Service**.

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-11a.png" >}}



...This is the result we were looking for, as it **ensures we aren't incorrectly deploying breaking changes to our live environment**. Let's have a quick look at the **"tests"** tab in **Appveyor** and see the result -

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-14.png" >}}



There's our failing test, but what about the artifacts...we're they created regardless of the failing unit test during the build process?

{{< figure src="/images/2019/04/simon-gilbert-dev-cto-xunit-ci-15.png" >}}

...No they weren't, which helps ensure we aren't accidentally deploying an artifact with failing unit tests.

Enjoy!