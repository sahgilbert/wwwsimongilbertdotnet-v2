+++
author = "Simon Gilbert"
date = 2021-06-16T15:41:00Z
description = ""
draft = false
image = "/images/2020/08/Screenshot-2020-08-13-at-16.08.56.png"
slug = "test-driven-development-tdd-typescript-lambda-api-mocha-chai"
summary = "Test Driven Development (TDD) has many benefits, including allowing us to focus on our requirements with a \"unit test first approach\" to ensure that we're only deploying code where the unit tests pass. Today we're going to cover using Mocha and Chai to test our Typescript Lambda algorithms!"
title = "Test Driven Development (TDD) Typescript Lambda API using Mocha and Chai"
tags = ["Typescript", "Serverless", "TTD", "Mocha", "Chai"]
+++


Test Driven Development (TDD) has many benefits, including allowing us to focus on our requirements with a "unit test first approach" to ensure that we're only deploying code where the unit tests pass. Today we're going to cover using **Mocha** and Chai to test our Typescript Lambda algorithms!For the purpose of this example, I've built a Serverless Lambda API that is designed to accept phone numbers, and determine what type of phone number it is - be it a UK mobile phone number, a UK landline number, or an invalid phone number. Our unit tests will focus solely on validating that the encapsulated service which performs this logic, is producing valid and correct results.

### Test Driven Development (TDD)

The cycle in the Test Driven Development (TDD) process is very simple, yet repetitive. First of all, you write a unit test which will fail because you haven't written any code to make the test pass. You then proceed to write code until the unit test passes. Once the unit test passes, you then refactor your code to make it more clean and scalable. You then apply this methodology to all parts of the system that you intend to unit test, ensuring that you write the unit test first, and then write the code to make the unit test pass, followed by the refactoring process. The cycle looks like this -

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-13.21.13.png" >}}

With this in mind, let's talk about Mocha and Chai. Mocha is a JavaScript testing framework running on NodeJS. Mocha allows asynchronous testing, test coverage reports, and use of any assertion library. Chai is a TDD/BDD assertion library for NodeJS that can be paired with any javascript testing framework. So essentially, Mocha is a testing framework and chai is an assertion library.Mocha uses hooks to organize the structure of its unit tests. They are as follows, and we'll cover a few of these in today's blog -

* describe(): Used to group and describe the test case.
* it(): Used to state the test case itself.
* before(): A hook to run before the first it() or describe().
* beforeEach(): A hook to run before each it() or describe().
* after(): A hook to run after it() or describe().
* afterEach(): A hook to run after each it() or describe().

### Installing Development Dependencies

Ok let's get started. Head to your source code project and run the following command using NPM, to install ts-node, mocha, chai and their equivalent types. These additional packages are type definitions for mocha and chai, and ts-node is a TypeScript execution environment for node. The command is (npm install chai mocha ts-node @types/chai @types/mocha --save-dev).

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.08.39.png" >}}

### Writing Our First Unit Test

We'll start with a simple "Hello world!" unit test. Create a new file called "HelloWorldService.spec.ts" (since we intend to build a HelloWorldService, and the ".spec.ts" suffix is indicative of a unit test file). Within your HelloWorldService.spec.ts file, at the top you'll want to import mocha and chai as follows, and then make use of the Mocha hooks (describe and it), to define a simple test whereby we expect to receive a string of "Hello world!" back from our HelloWorldService. I'm using the "describe" hook to define that this is a Hello world string function unit test, and the "it" hook to state what the expected result of the test is.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.09.16.png" >}}

Now that we've defined our unit test file and our simple "Hello world!" unit test, we'll need to create our HelloWorldService (in it's most basic form, without any sort of logic). This is simply to enable the unit test to run...and fail, in order to demonstrate the first steps of TDD. Here is a simple example - 

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.13.56.png" >}}

The next step is to create an NPM script in our package.json file, that calls mocha with the path as a parameter. However, instead of letting node run mocha, we’re going to register ts-node to run mocha instead. This will allow us to run our unit tests from the terminal, and the script looks as follows (you'll notice how the .spec filepath has been made use of, as stated previously).

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.16.25.png" >}}

We can now run the "npm test" script command from the Terminal - 

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.21.01.png" >}}

Here is our output result, and as expected, our unit test failed.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.22.26.png" >}}

Let's fix our HelloWorldService function to return the correct result.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.23.33.png" >}}

Now let's re-run the unit test script.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.24.57.png" >}}

Our unit test is now passing, and we've completed our first cycle of Test Driven Development (TDD)!!

### Unit Testing Our Phone Number Service Algorithm

Now that our Typescript Lambda project is configured for TDD using Mocha and Chai, we can begin building our phone number service algorithm to determine what type a phone number is defined as. For the purpose of brevity, this will be summarised. Let's concentrate on the part of the PhoneNumberService algorithm, that is designed to determine whether a phone number is a UK mobile phone number. The first thing we're going to do is define our unit test _(and you'll notice that in order to make the code compile and the unit test fail in the first instance, I've created a PhoneNumberService class and the relevant method/function stubs for us to define our unit test against)._

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.41.40.png" >}}

In this unit test, we have used the "describe" hook to define that this unit test is to determine what the phone number type is, and then we've used the "it" hook to state that this unit test should return a result of "UK_MOBILE_PHONE_NUMBER" (which is defined on an enumerated type). Now let's run our unit tests and see if the code compiles, and produces a failing test (as expected in the first instance).

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-14.50.30.png" >}}

As expected, our test is failing, and we can now write our PhoneNumberService algorithm to pass the unit test, and validate our input UK mobile phone numbers. For the purpose of this example, I've decided to use a Regular Expression (RegEx), in order to validate whether the input value is a UK mobile phone number. You'll note from the code below that we first declared a constant variable which defines the parameters for our Regular Expression, followed by creating a new instance of our RegEx, and then passing the input phone number into the "test" function of the RegEx instance.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.00.41.png" >}}

Now that we have defined our PhoneNumberService algorithm using a RegEx, we can re-run our unit tests!

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.02.48.png" >}}

As you can see, our unit test is now passing, and the PhoneNumberService Regex is determining correctly that the input phone numbers are indeed, valid UK mobile phone numbers. The next step is to add more unit tests for our algorithm, and then check to see if they pass or fail.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.15.03.png" >}}

The new unit tests are failing as expected, and we can now refactor our PhoneNumberService to be more validating with the data, at the point where it is first passed into the algorithm, and then expand it to handle UK landline numbers also as follows.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.17.07.png" >}}

Now that we have refactored our PhoneNumberService to handle both UK mobile and landline phone numbers, and also validate that an input phone number is of a valid phone number format, we can re-run our unit tests again.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.19.15.png" >}}

Success! All of our unit tests are now passing with our refactored PhoneNumberService algorithm code!Next, let's take a look at our Serverless Lambda code to observe the construction and use of the various services to validate and respond to API requests.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.19.52.png" >}}

As you can see I've built a ValidationService also, which validates that the income values via the Lambda API Gateway are parsable, followed by some objects to handle the request and response, including an Http Response and Status Code.

Now let's run "sls deploy" and deploy our Serverless Lambda API to AWS.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.24.27.png" >}}

Our Serverless API has successfully deployed to AWS, so let's use Postman to hit the endpoint and test our PhoneNumberService algorithm.

{{< figure src="/images/2020/08/Screenshot-2020-08-13-at-15.26.51.png" >}}

As you can see, we're receiving successful responses from the postback as expected (thanks to our TDD approach, which ensured that all of our unit tests passed before we deployed our code).

Enjoy!