+++
author = "Simon Gilbert"
categories = ["C#", "Chief Technology Officer", "C#.Net", "Comparison", "C Sharp", "CTO", "Dot Net", "Microsoft Visual Studio", ".Net Core", "Object Oriented Programming", "Performance", "Simon Gilbert", "Search", "Algorithm", "Computer Science", "Binary Search", "Linear Search", "Jump Search", "Computational Complexity Theory", "Code", "Coding", "Dev", "Programming", "Software Development", "Web Dev", "Web Development"]
date = 2019-02-20T20:17:27Z
description = ""
draft = false
image = "/images/2019/02/simon-gilbert-cto-tech-blog-post-four.png"
slug = "comp-sci-search-algorithms-csharp-dotnetcore"
summary = "Computational complexity theory anyone? Simon Gilbert compares sequential versus interval search algorithms for performance in .Net Core."
title = "Computer Science Search Algorithm Performance - C# .Net Core"
tags = ["C#.Net", "Algorithms"]
+++


### The Algorithmic Search Requirement

A common feature that is required in software is the ability to search through a portion of data to obtain a specific result. A typical example is when you've been asked to code an autocomplete input for an e-commerce website such as _takemetoawarmerclimate.com_...

### Computational Complexity Theory

> "The focus of classifying computational problems according to their inherent difficulty."

...with that in mind, one of the key computational problems within the Computer Science industry is the **search problem**, which is represented by binary relation (a subset of the Cartesian Product).

_A **search algorithm** is therefore any type of algorithm that solves the aforementioned search problem._

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-turing-machine.png" caption="Turing Machine Illustration" >}}

### Search Algorithm Classifications

Search Algorithms are designed to retrieve an element from a dataset. These algorithms are generally classified into two categories:

1. **Sequential Search**: Traverse the dataset sequentially, checking every element.
2. **Interval Search**: Applied specifically to a sorted dataset and calculated through a series of iterations. Each iteration divides the dataset and repeatedly targets the center of the dataset until the result is found.

### Linear Search (Sequential)

> "Given an array of **n** elements, perform a search to find a given element **x** within the array."

When performing a Linear Search, we start at the leftmost element of the array and check each element one by one to determine whether its value is equal to **x.**

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-linear-search.png" caption="Linear Search Algorithm" >}}

_The time complexity of Linear Search is_  **_O(n)_**.

### Jump Search (Interval)

> "Given a **sorted** array of **n** elements, perform a search to find a given element **x** within the array in fewer steps than a **Linear Search**."

Unlike Linear Search, Jump Search uses a sorted array. The algorithm is designed to check fewer elements than Linear Search by jumping ahead and skipping some elements (instead of searching all elements). Using our array and a block (to be jumped), we search the indexes until we find the interval, upon which we perform a linear search operation from the index to find the element.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-jump-search.png" caption="Jump Search Algorithm" >}}

_The time complexity of Jump Search is_  **O(√n).**

### Binary Search (Interval)

> "Given a _**sorted**_ array of **n** elements, perform a search to find a given element **x** within the array."

Executing a Binary Search differs from a Linear Search in that it uses a sorted array, much like a Jump Search. The algorithm is designed to repeatedly divide the search interval in half after each iteration, and begins with an interval that covers the entire array. If the value of the search key is less than the item in the middle of the interval, we narrow the interval to the lower half, otherwise we narrow it to the upper half. We therefore repeatedly check until the value is found or the interval is empty.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-binary-search.png" caption="Binary Search Algorithm" >}}

_The purpose of Binary Search is to reduce the time complexity to_  **_O(Log n)_**_, thanks to the use of a sorted array._

### Testing Our Implementations

I've put together a series of basic unit tests, to verify that each search algorithm implementation is providing the expected results across a series of permutations (finding the first, middle and last number within a dataset).

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-search-algorithm-unit-tests.png" caption="Search Algorithm Unit Tests" >}}

...looking good, all tests passing!

### To Lambda or Not...?

Over time, the C# language has been enhanced with new functionality for implementing, shortening, and enhancing your codes performance. One of my favourite additions to the language is **Lambda Expressions**, which I've used to refactor a new version of out **Linear Search** algorithm below:

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-linear-search-lambda.png" caption="Linear Search Using Lambda Expressions" >}}

### Computational Theory

Computability is the ability to solve a problem in an effective manner. The aim is to find the fundamental constituents that bind the solution to the equation. Certain problems require more time to compute...even though they are computable, and therefore the algorithmic choice is key, particularly when performance is a requirement.

### The Performance Test

The performance test involves 5 implementations of the 3 key search algorithms (2x Linear, 1x Jump, 2x Binary). We then group each performance test to involve finding either the first, middle, or last numeric in a dataset of 1000 numbers.

### Performance Test Results

In every search test, we can see that the **Binary Search** algorithm wins -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-search-algorithm-performance-results.png" >}}

### Why Did Binary Search Win?

...Firstly, note how the order of performance is always consistent, in that Linear is always slowest, Jump is always second, and Binary always wins. Let me explain why...

A **Linear Search** is designed to check every element which inherently takes time. The time taken will increase exponentially, the larger the dataset being checked.

A **Jump Search** is designed to check fewer elements than a **Linear Search** in order to compute the result...Fantastic! The advantage of a **Jump Search** over a **Binary Search** is that it only needs to jump backwards once, while a **Binary Search** can jump backwards up to log _**n**_ times, (this can be important if a jump backwards takes significantly more time than a jump forwards).

...However, a **Binary Search** will cut the required computation time in half as soon as it determines the middle of the sorted dataset, and thus the computation is applied to only half of the dataset specifically.

It is also worth noting that a **Linear Search** performs equality comparisons, while a **Binary Search** performs ordering comparisons.

Enjoy!