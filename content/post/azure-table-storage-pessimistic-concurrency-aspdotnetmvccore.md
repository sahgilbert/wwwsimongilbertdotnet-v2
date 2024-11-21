+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "Autofac", "Azure Blob Storage", "Azure Storage Explorer", "Azure Table Storage", "Blob", "Blob Storage", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dependency Injection", "Dev", "Dot Net", "Github", "Microsoft Azure", "Microsoft Visual Studio", "MVC Core", ".Net Core", "NoSQL", "Nuget", "Object Oriented Programming", "Programming", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Table Storage", "Concurrency", "Optimistic Concurrency", "Pessimistic Concurrency", "Concurrency Control", "ACID", "ACID Properties", "Locking", "Blob Lease"]
date = 2020-01-22T11:45:00Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-13.png"
slug = "azure-table-storage-pessimistic-concurrency-aspdotnetmvccore"
summary = "Azure Table Storage concurrency issues? Simon Gilbert provides a simple explanation of implementing pessimistic concurrency control using Azure Blobs in C# ASP.Net MVC Core"
title = "Microsoft Azure Cloud Table Storage - Optimistic vs. Pessimistic Concurrency using C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Azure Table Storage", "Azure Blob Storage", "Concurrency"]
+++


### Enter “Microsoft Azure Table Storage”...

So you've taken the plunge into using **NoSQL** database storage and are keen to adopt **Microsoft Azure’s** highly scalable (and cheap) **Table Storage** implementation. However, there’s a catch...it runs optimistic concurrency by default and depending on the data contention aspects of your platform, this can lead to undesired outcomes with your transactions.

{{< figure src="/images/2019/03/Screenshot-2019-03-28-08.11.47.png" >}}

### Optimistic Concurrency Control (OCC)

The optimistic concurrency model assumes that multiple transactions can frequently complete without interfering with each other. This process involves transactions using resources without acquiring locks on those resources, and just before the transaction commits the change, it checks to see if another transaction has modified the data since the initial transaction began to execute. In the event that a conflict has been detected, the transaction rolls back and can, or rather needs, to be restarted.

### The Downside Of Locking

At this point you’re thinking why would someone choose optimistic concurrency when conflicts are possible. Generally it’s used in scenarios with low data contention where conflicts are rare, but another key justification for using optimistic concurrency is that managing locks is expensive, particularly when a transaction has to wait for another transactions locks to clear which can then impact throughput...The downside of being pessimistic I guess?

### Pessimistic Concurrency Control (PCC)

In scenarios where contention is frequent, the performance impact of repeatedly restarting transactions is going to significantly impact your implementations ability to scale and deliver in a reasonable timeframe. The reverse approach is of course to use pessimistic concurrency, which takes its name from its nature of assuming the worst, the worst being two transactions trying to update the same record at the same time. Given this, it prevents that possibility by locking the record, no matter how unlikely the potential of conflicts occurring actually is. The pessimistic locking approach ensures that the record is updated successfully, but the downside of this strategy is that it ensures the record is locked and inaccessible by other processes throughout the time of the transaction.

### Scenario

You're building an ordering system whereby multiple instances of your order service can be accessed by different parts of the system, in parallel, to execute the task of updating the current status of the order. Your underlying database is **Microsoft Azure Table Storage**.

### Problem

As mentioned, by default you’re using **Azure Storage's** optimistic concurrency in a scenario with high data contention and the likelihood of updates failing to occur when conflicting transactions take place is probable. **Azure Table Storage** determines conflicts through use of an **ETag** which is updated every time the record itself changes. If the version of the **ETag** that you have does not match the latest version on the existing record, an **HTTP 409 Conflict** status occurs, and the transaction does not proceed.

### Solution

In this case, a different result can be achieved by implementing a pessimistic locking strategy into your process. The approach we're going to look at today is to use **Microsoft Azure's Blob Storage** in conjunction with **Table Storage**, to _acquire a lease on the blob_ that is associated with the table row temporarily whilst the updates occur, and thus ensuring a pessimistic locking approach is preventing conflicts. To make this as clean as possible in terms of structure, we're going to **dependency inject** the required parts into a separate service and repository using **Autofac**.

### Let's Code...

You'll need to pull in a copy of the **WindowsAzure.Storage** package from Nuget. Let's take a quick look at the **TableEntity** class, which our business logic layers underlying data model will need to inherit from in order to represent a table.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-tableentity.png" caption="Microsoft Azure Table Storage - TableEntity" >}}

As you can see, upon inheriting this class our data model will represent a table at the application layer with four specific properties, one of which is the previously mentioned **ETag**. We'll add a simple string value to our data model to represent the current "status" of the order.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-entity-inheritance.png" caption="Microsoft Azure Table Storage - TableEntity Inheritance" >}}

Now to build our **CRUD** layer, consisting of a repository with an **Azure**  **CloudTable** injected into it, followed by a service with the repository and an **Azure**  **BlobContainer** injected into it.

### Repository

Our repository stub is as follows, with our **CloudTable** injected.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-repository.png" caption="Microsoft Azure Table Storage - Repository With CloudTable" >}}

### Service

Our service stub follows a similar process, in that it injects both our repository instance and our **CloudBlobContainer** for the lease acquisition part of our pessimistic concurrency control.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-service.png" caption="Microsoft Azure Table Storage - Service With CloudBlobContainer" >}}

### Autofac Dependency Injection

I won't cover this part in too much detail, but essentially this configuration means we have only the required code statements for performing our concurrent CRUD operations within our service and repository classes.

Let's start with the **Azure** blob container injection.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-blob-dependency-inject.png" caption="Microsoft Azure Table Storage - Autofac Blob Dependency Injection" >}}

...Followed by our repository and table injection.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-table-dependency-inject.png" caption="Microsoft Azure Table Storage - Autofac Table Dependency Injection" >}}

### Next Steps

Now that we have the key parts dependency injected into our service and repository we can get to work on the **CRUD** meets pessimistic concurrency aspect. We'll start with a simple repository to create, update and get our data back from our **Azure Table Storage**.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-crud-layer.png" caption="Microsoft Azure Table Storage - CRUD Repository" >}}

Our service layer will be given a _basic_  **pessimistic concurrency control** strategy. As mentioned we are going to lock our **Azure Table Storage** row by acquiring a lease on an **Azure Blob Storage** item that has the same underlying unique RowId. Let's implement our mechanism to create new blobs on demand and retrieve them when necessary.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-blob-storage-lease.png" caption="Microsoft Azure Table Storage - BLOB Storage Service" >}}

Next is our service layer which will interact with our **Blob Storage** methods. This is a two-step process, in that when we create the record in our database table, we create an associated **Azure Blob** which we can acquire a lease on in future when performing updates, thus creating a "pessimistic lock" on our record.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-blob-lease-create.png" caption="Microsoft Azure Table Storage - Service Create Blob" >}}

Our update method is where the locking occurs, with our acquisition of the **Blob** lease, followed by the release of the lease condition upon completing our record update.

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-azure-blob-lease-acquire.png" caption="Microsoft Azure Table Storage - Service Acquire Blob Lease" >}}

Now of course this is a very simple example, but what we're doing here is ensuring that a pessimistic lock exists to help us adhere to the **ACID (Atomicity, Consistency, Isolation, Durability)** properties, meaning our database integrity remains.

In an ideal and more long term scenario, the implementation of a "wait and retry" mechanism would be necessary, which would mean that if an existing transaction has already locked the record when a subsequent transaction attempts to make an update, the subsequent transaction would indeed wait for a determined period of time before retrying to apply its update again, in the hope that the pessimistic lock from the previous transaction had been released. The more flexible this implementation is, the more strategic you can be with managing your performance and throughput of database transactions using **Microsoft Azure's Table Storage**.

...We can look at implementing that mechanism in a future blog post.

Enjoy!