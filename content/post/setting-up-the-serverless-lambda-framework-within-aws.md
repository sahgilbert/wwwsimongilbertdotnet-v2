+++
author = "Simon Gilbert"
date = 2020-08-07T11:40:00Z
description = ""
draft = false
image = "/images/2020/08/Screenshot-2020-08-12-at-12.38.06.png"
slug = "setting-up-the-serverless-lambda-framework-within-aws"
summary = "Serverless Lambda - I'm sure you've heard all about it and are keen to get coding some serverless functions, but before we get started, we need to configure AWS to allow us to create and deploy Serverless projects, so today we're going to cover getting setup with Serverless in AWS!"
title = "Setting Up The Serverless Lambda Framework Within AWS"
tags = ["AWS", "Serverless"]
+++


Serverless Lambda - I'm sure you've heard all about it and are keen to get coding some serverless functions, but before we get started, we need to configure AWS to allow us to create and deploy Serverless projects, so today we're going to cover getting setup with Serverless in AWS!

Create a new AWS account, and then log in to the AWS Management Console. Once you're in AWS, search for "IAM" (Identity Access Management) within the services search bar.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-11.52.10.png" >}}

Under "IAM", select Users.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-11.53.03.png" >}}

Within Users, select "Add User".

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-11.53.34.png" >}}

Create a new user by providing a username, and ensuring you select to add "Programmatic Access" (this allows Serverless to work with AWS via the relevant SDK's and CLI Tools).

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-11.58.16.png" >}}

Next choose "Permissions". Within Permissions, we are going to attach an existing policy directly to our user account. The permission to select in this case is "AdministratorAccess" (which will allow Serverless to carry out whichever tasks it needs to, includng creating AWS Lambdas, S3 Buckets etc.)

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.01.00.png" >}}

Continue to the "Tags" page. We don't need to set anything up in here to make Serverless work, so let's continue to the "Review" page.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.01.26.png" >}}

On the Review page, you will see that we now have a Serverless IAM user account, that has programmatic access and AdministratorAccess, so let's create our user.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.04.05.png" >}}

Now that our user has been created, we can see that we have an "Access Key ID" and a "Secret Access Key".

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.05.30.png" >}}

The next step is to copy these values and navigate to a Terminal window. Once inside Terminal, we need to firstly install Serverless using the following command (be sure to use -g, in order to install serverless globally on your system).

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.09.30.png" >}}

Now that Serverless is successfully installed locally, we can setup our credentials, to allow Serverless to work on our AWS account.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.11.43.png" >}}

Use the following command to apply our AWS credentials to our Serverless installation, which includes the Access Key ID and Secret Access Key from the newly created IAM User in AWS. You will also note that we add a "profile" configuration to the command, which allows us to apply a descriptive name to our Serverless Profile.

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.19.44.png" >}}

Serverless is applying our AWS credentials...

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.27.00.png" >}}

Now that Serverless is setup with our AWS credentials, we can use template commands to generate code and begin building Serverless software. A template that I commonly use is "serverless create --template aws-nodejs-typescript".

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.33.14.png" >}}

You can also define a path in your command, to create your serverless service within a dedicated folder as follows "serverless create --template aws-nodejs-typescript --path MyServerlessService".

{{< figure src="/images/2020/08/Screenshot-2020-08-11-at-12.34.48.png" >}}

Enjoy!

