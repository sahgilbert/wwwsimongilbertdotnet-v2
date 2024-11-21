+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Decoupling", "Design Patterns", "Message Patterns", "Software Patterns", "Dev", "Dot Net", "Github", "Microsoft Azure", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Nuget", "Object Oriented Programming", "Programming", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Service Bus", "Distributed Message Queue", "Message Broker", "Message Queue", "Publish-Subscribe Pattern", "Publish-Subscribe", "Pub-Sub", "Throughput", "Loose Coupling", "Topic", "Subscription", "Queue", "Namespace"]
date = 2019-04-01T10:03:03Z
description = ""
draft = false
image = "/images/2019/04/simon-gilbert-cto-tech-blog-17.png"
slug = "azure-service-bus-pub-sub-aspdotnetmvccore"
summary = "Intermittent throughput spikes? Simon Gilbert explains the publish-subscribe pattern with Microsoft Azure Service Bus using C# ASP.Net MVC Core"
title = "Microsoft Azure Service Bus - Distributed Message Queue (Publish-Subscribe) using C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Azure Service Bus", "Message Queue"]
+++


### Distributed Throughput Management

Let's imagine that you're in a situation where multiple components within your platform need to process messages at different rates. It's not uncommon for this use case to arrive in a modern day distributed system, so let's discuss "load leveling" using **Microsoft Azure Service Bus**.

{{< figure src="/images/2019/03/simon-gilbert-cto-azure-service-bus.png" caption="Microsoft Azure Service Bus" >}}



### Enter "Azure Service Bus"...

Essentially this is cloud **messaging as a service (MaaS)**. At the core it's a cloud hosted **message broker** for application **decoupling** which can be used to help you manage throughput and intermittent spikes in traffic, but also to **scale out** and distribute your messages to multiple receiving components (i.e. the **publish-subscribe pattern**). By using a queue, this ensures that the consumer only has to be provisioned to handle an average load instead of peak load on a constant basis.

### Simple Message Queue Breakdown

The most basic implementation of the **Azure Service Bus** messaging queue is made up of two components - A namespace and a queue. The queue is held within a particular namespace, which affectively acts as a container.

The queue includes a sender and a receiver and is used to allow messages to be sent to, and received from on an **endpoint-to-endpoint** basis. The queue does work on a first in, first out **(FIFO)** basis, and it will retain the messages until the receiving endpoint is capable of...receiving the messages. This implementation provides a **temporal decoupling** mechanism between your components, since the messages are stored in the queue until processed.

{{< figure src="/images/2019/03/simon-gilbert-cto-azure-service-bus-queue.png" caption="Microsoft Azure Service Bus - Message Queue" >}}



### Publish-Subscribe Pattern Breakdown

I'm sure you're familiar with messaging patterns, with the most well known of these being the **publish-subscribe pattern** which allows a one-to-many consumption of messages. The **Azure Service Bus** component for implementing this pattern makes use of what they refer to as **Topics and Subscriptions**.

{{< figure src="/images/2019/03/simon-gilbert-cto-azure-service-bus-topic.png" caption="Microsoft Azure Service Bus - Publish Subscribe Pattern" >}}

Today we're going to focus on the **publish-subscribe pattern** implementation, with the example being the publication of food orders for restaurants to a queue that is processing the data it receives.

### Let's Configure Azure...

Head to **Microsoft Azure and** choose the **Service Bus** option from the main menu.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-1.png" caption="Microsoft Azure Service Bus - Menu" >}}



...We're going to create a namespace to act as a container, so choose "Create Service Bus Namespace" next.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-2.png" caption="Microsoft Azure Service Bus - Create Namespace" >}}



For this implementation you'll need the "Standard" pricing tier at least, since the "**Basic**" tier doesn't support Topics (which we're dependent on for the **publish-subscribe pattern**). I live in London, so I've purposely chosen UK South as the location for the service bus hosting.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-3.png" caption="Microsoft Azure Service Bus - Create Namespace &amp; Pricing" >}}



**Azure** will now work its magic to get your service bus (namespace) choice deployed, and we can begin to configure it.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-4.png" caption="Microsoft Azure Service Bus - Namespace Deployed" >}}



The next option you want to choose is "**Topics**" from the **Azure Service Bus** menu.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-5.png" caption="Microsoft Azure Service Bus - Topics" >}}



...Let's give our topic a relevant name which aligns to the data we're going to be submitting to our service bus queue as a "**publisher**".

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-6.png" caption="Microsoft Azure Service Bus - Create Topic" >}}



Now that we have a "**topic**", we need to create a "**subscription**" for our consumers to use when hooking into our **queue**.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-7.png" caption="Microsoft Azure Service Bus - Create Subscription" >}}



Again let's give it a relevant name. As you can see "process-orders" will subscribe to the "restaurant-orders" topic".

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-8.png" caption="Microsoft Azure Service Bus - Configure Subscription" >}}



### Let's Code... (Our Publisher)

We'll begin by building our publisher within a C# ASP.Net MVC Core application. Pull in a copy of **Microsoft.Azure.ServiceBus** from **Nuget**.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-9.png" caption="Microsoft Azure Service Bus - Nuget Package" >}}



Our PublisherService is a wrapper around the **TopicClient** class from the aforementioned package. The client accepts messages asynchronously, which must contain a byte array as the message body. We're going to pass a class implementation as opposed to just a simple string, so we'll need to serialize the data first before converting it to a **UTF8** byte array.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-10.png" caption="Microsoft Azure Service Bus" >}}



Head back to **Microsoft Azure** and choose the **Shared Access Policies** option from the **Service Bus** menu. From here you can obtain the **Primary Connection String** value.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-sharedaccesspolicies.png" caption="Microsoft Azure Service Bus - Shared Access Policies" >}}



Next we're going to invoke the **SendAsync** method on our **TopicClient** instance and pass our Message to the queue, specifically for our chosen topic name.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-publisher.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



### Subscriber Implementation

Now that we are able to publish messages to our queue, we need to implement a service to consume this data. For simplicity, we'll be coding this as a C# .Net Core console application. Our "subscription" depends on the **Azure Service Bus Nuget** package **SubscriptionClient** class.

The first thing we need to do is register our message handler and pass in some options to configure how we would like it to behave. We'll start with some basic exception handling to print out any exceptions that we receive on the message pump to the console.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-12.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



Next we need to process our received messages. There are a few things to note here. Firstly, the cancellation token is passed as a parameter to the callback method in order to determine if our **SubscriptionClient** has already been closed. On receipt of our message, we also need to deserialize it back to the same class definition that was used to submit it to our queue.  For simplicity we're going to print our deserialized data model to the console, to indicate how the pub-sub queue works.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-13-1.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



Following this we need to configure some options. There are two main properties to assign values to at this stage - **MaxConcurrentCalls** which defines how many messages we want our consumer to process in parallel (I've kept this to 1 for now, for simplicity), and secondly the **AutoComplete** property which determines whether we want the message pump to complete the messages themselves after each one is received (which I've set to false in order to indicate that our messaging processing callback method should handle the message completion stage itself). We also need to inject our exception handling method into the constructor of our **MessageHandlerOptions** instance. Once this is done, we can invoke the **RegisterMessageHandler** method on the **SubscriptionClient** and pass in both our **MessageHandlerOptions** alongside our **ProcessReceivedMessages** callback method.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-14.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



Finally we can configure our SubscriptionService wrapper to run in our **C# .Net Core** console application. You'll need to pass in both the topic name and the subscription name to your **SubscriptionClient** instance, alongside the same **Primary Connection String** that we used for our **TopicClient PublisherService** from the **Azure Shared Access Policies** section.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-subscription.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



### Testing Our Azure Pub-Sub Implementation

Time to test our distributed message broker. I've injected the publisher into a controller and added a basic HTML view to post back some data to an action.

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-result-1.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}



Let's see the result...

{{< figure src="/images/2019/04/simon-gilbert-cto-dev-azure-service-bus-result-2.png" caption="Microsoft Azure Service Bus - Pub-Sub Pattern" >}}

...Pretty cool isn't it? The pub-sub pattern will therefore allow you to spin up multiple subscribers to handle your throughput in a loosely coupled manner. Now ideally to host our subscription endpoints we'd use something more long term such as a **Windows Service**, amongst other options. The **Microsoft Azure Service Bus** is also highly configurable, so next time we'll cover partitioned queues and the rest. Enjoy!

Enjoy!