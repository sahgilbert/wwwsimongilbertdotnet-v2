+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "C#", "Chief Technology Officer", "C#.Net", "CTO", "Dot Net", ".Net Core", "Object Oriented Programming", "Testing", "Tests", "Unit Testing", "Unit Tests", "XUnit", "Mocking", "Moq", "MVC Core", "Code", "Coding", "Computer Science", "C Sharp", "Dev", "Microsoft Visual Studio", "Programming", "Simon Gilbert", "Software Development", "Web Dev", "Web Development"]
date = 2019-03-02T12:58:49Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-seven.png"
slug = "aspnetmvccore-mocking-testing-moq-xunit"
summary = "Keen to take unit testing to the next level? Simon Gilbert walks through the benefits of using the Moq framework combined with xUnit to mock and isolate your unit tests using C# ASP.Net MVC Core in Visual Studio for Mac."
title = "Mocking & Testing using Moq & xUnit Frameworks for C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Unit Testing", "Moq", "XUnit"]
+++


### Isolation Matters

With the MVC pattern, controllers play a vital role in producing the behaviour that is expected when the website is running.

When it comes to unit testing, being able to isolate parts of the code from separate areas of the application can be necessary, particularly if the real version of the implementation itself is too awkward to test - a common issue with controllers.

### Mocking Things Out

Fortunately, we can create a "mock" version of an implementation which is able to act like the real thing, specifically for testing purposes.

Like everything in the tech industry, there are always multiple choices on offer...

### Enter "Moq"

Pronounced "mock", and to quote -

> "Moq is the most popular and friendly mocking framework for .Net"

Moq is designed to allow you the developer to manipulate your units of code and separate out cumbersome dependencies for clear isolated testing...and it supports .Net Core!

### A Typical Scenario

In a typical ASP.Net MVC Core implementation, a controller will have a dependency which has been injected at constructor level, and is being utilized within an action to return a view model of data to the relevant view.

### Our Dependency Code

Firstly, let's put together a basic service class with an interface -

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-dependency-for-mocking.png" caption="ASP.Net MVC Core Dependency" >}}

### Our Controller & Action Code

Next, we can inject our service into the controllers constructor, define our Index action and return the data to the view -

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-controller-for-mocking.png" caption="ASP.Net MVC Core Controller" >}}

### Mocking & Testing Our Controller Action Unit

Based on the aforementioned scenario, our steps for mocking & testing a specific controllers action method would be -

1) Create a mock version of our dependency.

2) Call the Setup method to explicitly state how the dependency should behave when it's called.

3) Inject our mock dependency into our controller.

4) Act out the action method.

5) Assert that our results are valid.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-post-moq-xunit-test.png" caption="ASP.Net MVC Core Moq xUnit Test" >}}

### Analysis

Our **HomeController**  **Index** action is designed to return all of the user accounts from our **UserAccountService** (a total of 2), map the models to their associated view models, and return the data to the **View**.

On **Line 22** we define the expected number of user accounts, whilst on **Line 36** we assert that a ViewResult type has been returned upon acting. On **Line 38**, we assert that the data has been mapped to the associated view model, before finally asserting that the expected count is correct on **Line 41**.

Enjoy!