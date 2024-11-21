+++
author = "Simon Gilbert"
date = 2021-04-14T17:53:00Z
description = ""
draft = false
image = "/images/2020/08/Screenshot-2020-08-16-at-18.06.33.png"
slug = "integration-testing-typescript-lambda-api-jest-supertest"
summary = "Integration Testing is a level of software testing where individual units are combined and tested as a group. The purpose of this level of testing is to expose faults in the interaction between integrated units. Today we're going to look at doing Integration Testing using Jest and Supertest."
title = "Integration Testing a Typescript Lambda API using Jest (& Supertest)"
tags = ["Typescript", "Serverless", "Integration Testing", "Jest", "Supertest"]
+++


Integration Testing is a level of software testing where individual units are combined and tested as a group. The purpose of this level of testing is to expose faults in the interaction between integrated units. Today we're going to look at doing Integration Testing using Jest and Supertest.

### What is Jest?

Jest is a JavaScript Testing Framework with a focus on simplicity. One thing I particularly like about Jest is that it aims to work out of the box, config free, which is always a joy rather than having to spend ages configuring your testing framework! Interestingly, to make things quick Jest runs previously failed tests first and re-organizes runs based on how long test files take.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.36.05.png" >}}

### Let's Code...

I'm going to assume that you already under AWS Lambda functions and API Gateway. Today I'll be using my WIP phone number api codebase to demo Jest, and writing our integration tests in Javascript.The first thing to do is install **Jest** and **Supertest**.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.38.43.png" >}}

Next, create a directory in your root folder called "**__tests__**".

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.39.26.png" >}}

Within that directory create another folder called "integration".

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.41.15.png" >}}

Now create a Javascript file that includes the name of the lambda function that we are writing integration tests for, such as **"lambda-name-here.test.js**".

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.41.33.png" >}}

Now head to package.json and create a new script to run our integration tests.



{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.45.28.png" >}}

Next let’s look at our integration test file, which includes our first test.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.54.39.png" >}}

We begin by creating a localhost url and port string, which we pass into Supertest when we import it into te file. Jest uses a familiar "describe" block, with "it" blocks inside of it. We are using Supertest to execute an HTTP Post request, which includes are body of JSON data to execute against our Lambda API endpoint. The callback inside the .then will be handled by Jest, so we can use any of the Jest expect methods in that callback.

### Running Our Integration Tests

Firstly, run the "serverless offline" command, so that we can test our API locally.



{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.55.11.png" >}}

Now run our integration test script "npm run integration".

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.55.23.png" >}}

Here's the result of our integration test, which has successfully passed.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.56.26.png" >}}

Let's add a few more integration tests to validate different scenarios, and review the results.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-17.57.43-1.png" >}}

As you can see, all of our integration tests with Jest are passing successfully, and our Lambda API endpoint is good to go!

Enjoy!