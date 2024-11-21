+++
author = "Simon Gilbert"
categories = ["C#", "Chief Technology Officer", "C#.Net", "C Sharp", "CTO", "Dot Net", "Github", "Microsoft Visual Studio", ".Net Core", "Object Oriented Programming", "Performance", "Simon Gilbert", "Parallelism", "Parallel.ForEach", "TPL", "Task Parallel Library", "Programming", "Code", "Coding", "Computer Science", "Cross-Platform", "Dev", "Software Development", "Web Dev", "Web Development"]
date = 2019-02-15T14:21:00Z
description = ""
draft = false
image = "/images/2019/02/simon-gilbert-cto-tech-blog-post-three.png"
slug = "parallel-haversine-formula-dotnetcore"
summary = "Convinced that parallelism is always faster? Simon Gilbert compares parallel computations of the haversine formula, versus simple sequential execution in .Net Core."
title = "Parallel Computations of The Haversine Formula - C# .Net Core"
tags = ["C#.Net", "Parallel Programming", "Haversine"]
+++


### The Spherical Distance Between Us

A common feature for modern applications is the need to supply the user with an accurate navigational distance between themselves and another endpoint, especially when providing an ETA for an item they have purchased using said piece of software.

### The Law of Haversines

Navigation is driven heavily by the law of haversines - a specific branch of spherical trigonometry which focuses on the relationships between sides and angles, particularly when it comes to polygons.

### The Haversine Formula

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-haversine-formula-equation.png" caption="The Haversine Formula" >}}

The haversine formula is the calculation of the great-circle distance, specifically using the latitude and longitude of two endpoints.

The great-circle distance (also known as the orthodromic distance) is the shortest distance between two points on the surface of a sphere. The key thing to note is that this distance is measured along the surface of the sphere (as opposed to a straight line directly through the sphere's interior).

### Haversine Formula Implementation

I'll refrain from delving into too much detail on the mathematical side of the Haversine formula. The following image shows the implementation we are going to use when running this calculation in C# .Net Core.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-haversine-formula.png" caption="The Haversine Formula in C# .Net Core" >}}

### Parallel Mathematical Computation

With the current hardware available in the market, we are able to utilise multi-core processors to ensure we maximise throughput and response times. Since version 4.0 of the .Net Framework, this has been made more accessible using the parallel extensions component of .Net which includes the Task Parallel Library (TPL), particularly when it involves loops as a method of execution.

The Parallel.For loop uses a ThreadPool to execute the work by invoking a delegate once per each iteration of the loop.

### Thread Synchronization Costs

It's worth highlighting that when we choose to use parallel loops, we incur a number of overhead costs -

* The cost of partitioning the source collection.
* The cost of  synchronizing the worker threads.

### Our Workload Dataset

For this example, I've produced a list of latitudes and longitudes that span various cities. Let's see how the dataset is handled across a variety of different parallel computation methods!

### "Parallelism Always Beats Sequential Execution"

....and here are the results:

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-haversine-results.png" caption="C# .Net Core Haversine Formula Results" >}}

The simple sequential execution method of a basic ForEach loop beats all the parallel implementations we put together, by a large number of milliseconds!

...So, are you still convinced that parallelism is always faster than sequential execution?

### When To Use Parallelism

It's important to note that performing parallel execution is not always faster than standard serial execution, and before deciding to performance optimise through concurrency, it is necessary to estimate the workload per iteration. If the actual work being performed by the loop is small (relative to the thread synchronisation cost), then it will be more performant to use serial execution...otherwise you're just adding unnecessary overhead.

Enjoy!