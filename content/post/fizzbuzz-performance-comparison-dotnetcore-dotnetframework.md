+++
author = "Simon Gilbert"
categories = ["C#", "Chief Technology Officer", "C#.Net", "C Sharp", "CTO", "Dot Net", "Github", "Microsoft Visual Studio", ".Net Framework", "Object Oriented Programming", "Simon Gilbert", ".Net Core", "FizzBuzz", "Performance", "Comparison", "Programming", "Dev", "Software Development", "Web Dev", "Web Development", "Code", "Coding"]
date = 2018-10-06T13:14:00Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-two.png"
slug = "fizzbuzz-performance-comparison-dotnetcore-dotnetframework"
summary = "Keen to understand performance improvements in .Net Core 2.2? Simon Gilbert provides a performance comparison of different FizzBuzz implementations using the .Net Framework vs .Net Core...The results are quite surprising!"
title = "FizzBuzz Performance Comparison - C# .Net Core vs .Net Framework"
tags = ["C#.Net", "Performance"]
+++


### As Jeff Atwood Once Said…

> “199 out of 200 applicants for every programming job can't write code at all.”

...I certainly don’t disagree with this statement, but let’s ignore that for the minute and discuss the idea of carrying out a performance comparison for various FizzBuzz implementations in C# .Net Core vs .Net Framework.

### What’s “FizzBuzz”?

The requirements for FizzBuzz are to write a program that prints the numbers from 1 to 100. But for multiples of three print "Fizz" instead of the number and for the multiples of five print "Buzz". For numbers which are multiples of both three and five print "FizzBuzz".

### FizzBuzz Comparisons

Today we’re going to compare three versions of FizzBuzz in .Net Core 2.2, vs. the equivalent three implementations using .Net Framework 4.7.2.

### FizzBuzz Method One - Super Simple

Our first implementation of FizzBuzz is super simple and provides nothing clever from a software development perspective, other than ensuring some very minor use of the Don't Repeat Yourself (DRY) rule. By using a For loop with a conditional statement, this implementation provides us with the correct results and is nice and easy to read.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-fizzbuzz-one.png" caption="FizzBuzz Version 1 in C# .Net Core" >}}

### FizzBuzz Method Two - Tuples & LINQ

In version two of our FizzBuzz implementation, we use tuples to associate the number 3 to the word "fizz", and the number 5 to the word "buzz". We then use LINQ to determine what to print for the current iteration, and whether a combination of Fizz & Buzz is required.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-fizzbuzz-two.png" caption="FizzBuzz Version 2 in C# .Net Core" >}}

### FizzBuzz Method Three - Single Line LINQ Statement

In our third and final FizzBuzz implementation, we use a single line LINQ statement to create the desired output. If you're used to coding LINQ it's fairly readable, and is different enough in design to our previous two versions which will give greater perspective on code execution time.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-fizzbuzz-three.png" caption="FizzBuzz Version 3 in C# .Net Core" >}}

### The Hardware Stack

I'm running each implementation on a MacBook Pro (Retina, 15-inch, Mid 2015) with MacOS Mojave, dual-booted with Microsoft Windows 8.1 using VMWare Fusion 7.1.3.

We will be running both versions on Microsoft Windows 8.1 only for a fair comparison.

### .Net Framework Results

The results are in...Version 1 took approximately 62 milliseconds, Version 2 took substantially longer with 241 milliseconds, and Version 3 was close in second place with 65 milliseconds.

_**Version 1, the super simple algorithm provided the fastest code execution for our C# .Net Framework implementation.**_

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-fizzbuzz-dotnetframework-result.png" caption="FizzBuzz .Net Framework Results" >}}

### .Net Core Results

Our .Net Core results differ substantially to our .Net Framework results.

Version 1 took approximately 43 milliseconds, with Version 2 managing to execute in a total of 73 milliseconds, and finally Version 3 produced the fastest code execution with a super speedy 17 milliseconds!

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-fizzbuzz-dotnetcore-resultt.png" caption="FizzBuzz .Net Core Results" >}}

### Framework vs. Core Result Comparison

The first thing to note here is the huge reduction in milliseconds for each of the three versions, when migrating from using the .Net Framework to .Net Core.

* Version 1 was 19 milliseconds faster.
* Version 2 was 168 milliseconds faster.
* Version 3 was 48 milliseconds faster.

The next thing that stands out is that Version 2 is always the slowest, regardless of whether we're running it as a .Net Framework or .Net Core implementation.

Now here's the interesting part - Version 1 and Version 3 swapped positions when we migrated to .Net Core, and this is particularly surprising given that Version 3 is a one line code statement written in LINQ, which (when used incorrectly) is known for underperforming...**_or is it?_**

### C# .Net Core LINQ Performance

So, as part of the .Net Core release, some major improvements were made to the Base Class Library, which of course covers LINQ (Language Integrated Query).

LINQ operators are lazy and therefore nothing happens until the result enumeration occurs. Along with other operators in LINQ, the **SELECT** statement has been performance optimised in .Net Core. Given that **SELECT** produces a sequence which has the same number of items that the source sequence does, .Net Core can therefore take advantage of this. The .NET Framework implementation however, performs a rather naive enumeration of the sequence produced by the **SELECT** operator, which of course takes much longer than the optimised version.

### Conclusion

As you can see from the results - The performance improvements that are gained from using .Net Core are substantial, and every optimisation that's been released is capable of consistently shaving off milliseconds, even in scenarios where you may not expect this to happen!

Enjoy!