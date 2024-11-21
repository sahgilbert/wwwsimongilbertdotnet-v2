+++
author = "Simon Gilbert"
date = 2020-11-19T18:14:00Z
description = ""
draft = false
image = "/images/2020/08/Screenshot-2020-08-16-at-19.23.50.png"
slug = "configuring-dynamodb-using-serverless-yaml"
summary = "DynamoDb is a brilliant NoSQL database from AWS. It integrates directly with the Serverless Framework, so today we're going to configure Serverless to work with DynamoDb"
title = "Configuring DynamoDb using Serverless YAML"
tags = ["AWS", "Serverless", "YAML", "DynamoDb"]
+++


DynamoDb is a brilliant NoSQL database from AWS. It integrates directly with the Serverless Framework, so today we're going to configure Serverless to work with DynamoDb.

First of all, let's add a stage of "dev" and a region of "us-east-1", followed by our Serverless profile name to our Serverless.yml file.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-19.31.55.png" >}}

Next let's add an environment variable to hold our DynamoDb table name, which is designed in a way to be based on the service name, the current deployment stage, and then the table name itself. This allows us to ensure we create separate DynamoDb tables per deployment environment.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-19.32.08.png" >}}

The next step is to define our IAM Role statements, which define the actions that will be available to our Lambda functions. Finally let's restrict our IAM role permissions to this specific table (for the current stage that we are deploying to.)

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-19.32.22.png" >}}

Next we'll create a resource to generate our DynamoDb table. Things to note here are that we have added a DeletionPolicy of Retain, to prevent us from accidentally deleting our Serverless DynamoDb table resources. Under the properties section you will also see that we've explicitly stated that we want to use our environment variable for the table name, as previously defined. Below this we've defined our primary key, and finally our BillingMode to be PAY_PER_REQUEST, since we are just starting out with our DynamoDb implementation.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-19.32.43.png" >}}

Finally let's run SLS Deploy, and head to AWS Management Console to review our deployment.

{{< figure src="/images/2020/08/Screenshot-2020-08-16-at-19.33.58.png" >}}

As you can see, Serverless has created our DynamoDb table based on our Serverless.yml configuration.

Enjoy!

