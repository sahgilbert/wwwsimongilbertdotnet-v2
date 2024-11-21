+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "Autofac", "Azure Table Storage", "C#", "Chief Technology Officer", "C#.Net", "C Sharp", "CTO", "Decoupling", "Dependency Injection", "Dot Net", "Inversion Of Control", "IoC", "Microsoft Azure", "Microsoft Visual Studio", ".Net Core", "Blob Storage", "Azure Blob Storage", "Blob", "Image Upload", "Azure Storage Explorer", "Code", "Coding", "Dev", "Object Oriented Programming", "Programming", "Simon Gilbert", "Software Development", "Web Dev", "Web Development"]
date = 2019-02-25T01:00:00Z
description = ""
draft = false
image = "/images/2019/02/simon-gilbert-cto-tech-blog-post-six.png"
slug = "azure-blob-storage-image-upload-aspnetmvccore"
summary = "Ever used Microsoft Azure Blob Storage? Simon Gilbert provides a detailed explanation on how to upload multiple images to Microsoft Azure Blob Storage using C# ASP.Net MVC Core in Visual Studio for Mac."
title = "Microsoft Azure Cloud Blob Storage Image Upload - C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Azure Blob Storage"]
+++


### Azure Cloud Hosted Image Upload

In the web development world, users often need to upload multiple images to the platform you're building (and hosting).

Storing and manipulating images can be expensive...thus, enter **Microsoft Azure Blob Storage**, deemed as massively scalable object storage for unstructured data.

> "Another day, another server, another rack? Not anymore. Blob storage can handle all your unstructured data, scaling up or down as your needs change. You no longer manage it, you only pay for what you use and save money compared to on-premises storage options."

### What's a "Blob"?

A Binary Large OBject (**BLOB**) is a collection of binary data stored as a single entity in a database management system. **Blobs** are typically images, audio or other multimedia objects****.****

For clarity, **Blob Storage** is used to store unstructured data such as text or binary data that can be accessed using HTTP or HTTPS from anywhere in the world.

### To The Microsoft Azure Portal We Go!

Whilst you can develop your codebase against **Azure Storage** locally, I decided to show you the full stack real-world implementation using an actual **Microsoft Azure** account.

_(If you don't already have one, it's well worth signing up for before we get started)._

### Deploying Azure Storage

Now that we're logged into the [Microsoft Azure Portal](https://portal.azure.com), head to the main menu and choose **CREATE A RESOURCE > STORAGE ACCOUNT**.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage.png" caption="Create Microsoft Azure Storage Account" >}}

Create a new resource group, give your storage account a relevant name, choose your server location (I live in London, UK so I've specifically chosen Azure's UK South server farm). I'll skip the explanation regarding replication and access tiers for the minute, but essentially in this example I've gone for a "hot" access tier, specifically for speed purposes. Click "Review + Create" and your storage will begin deploying to your chosen location.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage-deployed.png" caption="Microsoft Azure Storage Deployment Complete" >}}

...Now that our Azure Storage deployment is complete, we can begin writing some code.

### Azure Blob Image Upload Process

Uploading images from ASP.Net MVC Core to Azure Blob Storage is a multi-step process. For ease of use later on, I purposely store the key image details in Azure Table Storage, in parallel with uploading the image to Azure Blob Storage. The end-to-end steps are as follows -

1. Convert image to a byte array.
2. Upload image byte array to Azure Blob Storage.
3. Map the image details (name, url, content type) to an Azure TableEntity model.
4. Create a new row in Azure Table Storage for the image model TableEntity, for ease of accessing the image data when required later on.

We want to allow for multiple images to be uploaded, so we'll make use of the IFormFileCollection interface  which represents the collection of files sent with the HttpRequest.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage-blob-upload.png" caption="ASP.Net MVC Core Azure Blob Image Upload" >}}

As we iterate through each image, we can begin the upload process, starting with converting the image to a byte array -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage-blob-convert-byte-array.png" caption="ASP.Net MVC Core Azure Blob Image Byte Array Conversion" >}}

Following this we can upload our image byte array to Azure Storage as a Blob. You'll note that we're making use of the Azure CloudBlockBlob class and it's built in method for uploading an image byte array asynchronously to our storage account.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage-blob-upload-byte-array.png" caption="ASP.Net MVC Core Azure Blob Image Byte Array Upload" >}}

Finally, we can map the relevant image details to an Azure TableEntity class, and create a row in Azure Table Storage for easy access to our image data as and when necessary -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-table-storage-row-create.png" caption="ASP.Net MVC Core Azure Table Storage TableEntity Create" >}}

### What Happened To "HTTPPostedFileBase" in .Net Core?

You might be wondering what **IFormFileCollection** is, and what happened to **HTTPPostedFileBase** in **.Net Core**...

...Long story short, **HTTPPostedFilebase** was an abstract class that came as part of System.Web for the .Net Framework, which isn't available in .Net Core unfortunately. So instead, we can use **IFormFile** and **IFormFileCollection**.

### Azure Storage Access Keys

Head back to the Microsoft Azure Portal, head to your newly created storage resource and grab a copy of the connection string -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-storage-connection-sring.png" caption="Microsoft Azure Storage Access Keys" >}}

Next we need to add the connection string to the app settings file in our ASP.Net MVC Core application.

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-visual-studio-appsettings.png" caption="ASP.Net MVC Core appsettings.Development.json File" >}}

(I've specifically chosen to use the development version of this file, given this isn't a production release).

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-aspnetmvccore-appsettings.png" caption="ASP.Net MVC Core appsettings Azure Storage Connection String" >}}

### Dependency Injecting Our Service & Repository

Remember your SOLID Principles?…Good, we need to configure our ASP.Net MVC Core website to dependency inject our service and repository, since ultimately, this will allow us to bolt the functionality into any part of the website in a “plug and play” manner...

We'll start with the storage account first and pull in our connection string from our appsettings file -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-autofac-dependency-injection-1.png" caption="Autofac Dependency Injection Azure Storage" >}}

Let's inject our storage tables next...

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-autofac-dependency-injection-2.png" caption="Autofac Dependency Injection Azure Storage Tables" >}}

Following this, we can inject our blob containers -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-autofac-dependency-injection-3.png" caption="Autofac Dependency Injection Azure Storage Blob Containers" >}}

Finally we can bind our blob container and cloud table to our service and repository -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-autofac-dependency-injection-4.png" caption="Autofac Dependency Injection Azure Storage Binding" >}}

…the beauty of the “inversion of control (IOC)” principle!!!

### Ok...To The Cloud!

Let's run our ASP.Net MVC Core application and upload some images to the Microsoft Azure Cloud!

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-blob-image-upload-1.png" caption="ASP.Net MVC Core Azure Cloud Blob Storage Image Upload" >}}

Next we choose some images to upload -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-blob-image-upload-2.png" caption="ASP.Net MVC Core Azure Cloud Blob Storage Image Upload" >}}

As you can see, our use of IFormFileCollection allows us to upload multiple images simultaneously -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-blob-image-upload-3.png" caption="ASP.Net MVC Core Azure Cloud Blob Storage Image Upload" >}}

Finally, the result -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-blob-image-upload-4.png" caption="ASP.Net MVC Core Azure Cloud Blob Storage Image Upload" >}}

As you can see, when we inspect the uploaded images, they do indeed have the relevant **Azure Blob Storage** url -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-blob-storage-image-url.png" caption="ASP.Net MVC Core Azure Cloud Blob Storage Image URL" >}}

### Microsoft Azure Storage Explorer

When working with **Microsoft Azure Cloud Storage**, it's important to have a local visible copy of your data. Microsoft have indeed created a local cross-platform application that allows you to easily manage your storage anywhere using Windows, macOS and Linux.

Grab yourself a copy of **Azure Storage Explorer** (it's free), and let's have a quick look at our Azure Storage database table and blob container...

As you can see, I've run my local copy of Storage Explorer, and signed into my Microsoft Azure account, which has pulled my recently created storage account into the explorer application for me to view -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-create-azure-storage-explorer.png" caption="Microsoft Azure Storage Explorer" >}}

Let's view our blob container to see our uploaded images -

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-storage-explorer-blob-1.png" caption="Microsoft Azure Storage Explorer Blob Containers" >}}

Finally, lets view our azure cloud table that contains the relevant uploaded image details...

{{< figure src="/images/2019/02/simon-gilbert-cto-tech-blog-post-azure-storage-explorer-table-1.png" caption="Microsoft Azure Storage Explorer Cloud Table" >}}

Pretty cool right? You're now able to upload multiple images to the Microsoft Azure Cloud from a cross-platform application!

_NB - I've purposely left the access keys in the Github source code example for clarity, albeit having taken the deployed storage offline._

Enjoy!