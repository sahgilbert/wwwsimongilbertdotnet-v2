+++
author = "Simon Gilbert"
date = 2021-08-18T12:30:00Z
description = ""
draft = false
image = "/images/2020/08/Screenshot-2020-08-14-at-12.57.13-1.png"
slug = "continuous-integration-ci-cd-typescript-serverless-lambda-api-circleci"
summary = "Are you getting tired of manually deploying your code to AWS after each code change? Today we're going to implement Continuous Integration for Serverless Lambda, to automate our deployments using a CI/CD pipeline, based on the Git branch we push the commit to!"
title = "Continuous Integration (CI/CD) for a Typescript Serverless Lambda API using CircleCI"
tags = ["Typescript", "AWS", "Serverless", "CI/CD", "CircleCI"]
+++


Serverless is an amazing piece of technology, but are you getting tired of manually deploying your source code to AWS after eac code change? Today we're going to discuss how to implement Continuous Integration for Serverless Lambda, to automate our deployments using a CI/CD pipeline, based on the Git branch we push the commit to!

You'll remember in one of the previous blogs, we built a Serverless Lambda API that determined the type of phone number that it received, which we unit tested the algorithm for adequately. Today we'll fork a copy of that repository, and add the necessary configuration to deploy our existing codebase to AWS in an automated pipeline. Firstly, let's discuss what Continuous Integration is and how it is useful to your development lifecycle.

### Continuous Integration Explained

Continuous Integration (CI) is a development practice whereby developers integrate code into a shared code repository frequently throughout the day. Each integration can then be verified by an automated build and automated tests, before being deployed to a server. One of the key benefits of integrating regularly is that you can detect errors quickly and locate them more easily. As each change introduced is typically small, pinpointing the specific change that introduced a defect can be done quickly.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-10.51.10.png" >}}

There are a few different terms to be aware of, namely Continuous Integration and Continuous Deployment/Delivery. Continuous Integration is the practice of integrating changes from different developers into the code repository. This makes sure the code individual developers work on doesn’t divert too much. Continuous Deployment/Delivery refers to keeping your application deployable at any point (and even deploying to an environment if the latest version passes all automated tests).

### CircleCI

CircleCI is a Continuous Integration and Continuous Delivery (CI/CD) platform which runs as a PAAS. CircleCI allows your development team to automate your development process with CI hosted in the cloud or on a private server, which is pretty amazing! With CircleCI, teams get faster builds, shorter feedback cycles, and simplified pipeline maintenance. I personally am a fan of the fully hosted cloud service version of CircleCI, whereby they oversee the setup, security, and maintenance of your continuous integration instance(s). The long story short - This is a fantastic piece of kit for automating your Serverless deployments to AWS in a slick and easy manner.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-10.52.51.png" >}}

### Sign Up to CircleCI

First of all, you need a CircleCI account which is free. You can log in using your Github or Bitbucket account, and it will display all of your code repositories.

### CircleCI Setup

Now that you're logged in to CircleCI, choose the repository that you want to use as part of your continuous integration pipeline, and click "Set Up Project".

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.02.02.png" >}}

Following this click "Use Existing Config" (we will add this later).

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.04.24.png" >}}

The next step is to choose "Start Building" (you can ignore that we haven't added the required config.yml file yet, we simply want to get CircleCI to run a build on our repository, in order to configure the initial setup that is required to make the builds trigger automatically when we push git commits to our source code repository).

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.04.43.png" >}}

This build will fail, as expected (due to not having a config.yml file), but then we can add the required configuration for CircleCI.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.09.28.png" >}}

### Context and Environment Variables

Following this, select "Organization Settings" from the side menu.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.10.41.png" >}}

You can now choose "Create Context", which will allow us to add some shared Environment Variables to be used within our organisation (namely our AWS credentials for deploying our cloud services).

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.11.51.png" >}}

Let's give our context a relevant and unique name...

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.22.13-2.png" >}}

We can then choose our newly created context, and "add Environment Variables" for our AWS credentials (I'll assume you already have these setup). IMPORTANT - Ensure you name the environment variables exactly as they are in AWS, i.e. AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY, otherwise CircleCI will not be able to parse them later on.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.22.54.png" >}}

Our AWS credentials are now setup as Environment Variables in our CircleCI Organisation Context, so let's continue.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.26.04.png" >}}

### Let's Get Coding...

Open a new Terminal within the root folder of your source code repository, and create a new hidden directory called ".circleci", followed by changing into this newly created directory and creating a file called "config.yml" and another file called "deploy.sh".

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.36.07.png" >}}

We now need to make a modification to our newly created "deploy.sh" file, to ensure that CircleCI can read and execute the file. We will be using the command "chmod 755 deploy.sh", which we will run in the Terminal against the bash script file. When you perform the "chmod 755 filename" command you allow everyone to read and execute the file, and the owner is allowed to write to the file as well.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.39.47.png" >}}

The next step is to add the relevant code to our bash script, which will have two main parts. The first part will run the "sls config" command to setup our AWS credentials from our Environment Variables, within Serverless itself. The second part will run a switch statement, to specifically deploy to a certain environment (be in DEV, UAT or PROD), depending on the Git branch that we pushed to.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.43.21.png" >}}

Now that our bash script is setup, the next step is to configure our config.yml file, to allow CircleCI to deploy our Serverless Lambda API project. There are two parts to this file as well. The first part creates a Docker image to containerise our deployment, install Serverless and our dependencies, run our unit tests, and then execute our newly created bash script (deploy.sh). Here is the first part of the config.yml file, as described.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.50.28.png" >}}

The second part of the config.yml file allows us to apply our CircleCI Organisation Context (which contains our Environment Variables for our AWS credentials), followed by ensuring that the build only happens on our master, uat and develop Git branches in our source code repository. Ensure you add the correct name of your CircleCI Organisation Context to this section of the config.yml file.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.50.51.png" >}}

### Git Commit, Push and Production Environment Deploy

Now let's push these changes to our Git repository master branch, and we will see that CircleCI kicks off a new build specifically for the master branch.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.56.28-1.png" >}}

The build has finished, so let's review the results.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-11.59.02.png" >}}

As you can see, all the build and deploy steps were successful, so let's take a look at the final deploy step and see the details.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-12.02.52-2.png" >}}

The deployment step shows that CircleCI has successfully deployed our Serverless Lambda API to our AWS Production environment, so let's take a look at our CloudFormation Stacks within AWS and confirm this.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-12.03.16.png" >}}

As you can see, CircleCI has created our "phone-number-api-prod" CloudFormation Stack, so let's head to PostMan and test the newly deployed production Serverless Lambda API.

{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-12.04.00.png" >}}

### Development Environment Deployment Test

PostMan returns a successful result, which is fantastic. Let's try one more thing and test the development environment deployment also. Head back to your repository, create a new branch called "develop", push the changes to Git, and then wait for CircleCI to build and deploy the repository to AWS. Once this has finished, head to the AWS management console and view the CloudFormation Stacks page again, and let's see the results.



{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-12.12.03.png" >}}

As you can see, our Continuous Integration pipeline using CircleCI has deployed our "develop" branch in Git to our DEV environment on AWS, which is brilliant! Here is the final outcome in our CircleCI pipelines user interface.



{{< figure src="/images/2020/08/Screenshot-2020-08-14-at-12.16.14.png" >}}

You can see here that both our **MASTER** and **DEVELOP** branches have successfully deployed to **AWS**! 

Enjoy!