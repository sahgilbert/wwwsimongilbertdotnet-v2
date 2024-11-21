+++
author = "Simon Gilbert"
date = 2020-01-02T22:58:06Z
description = ""
draft = false
image = "/images/2020/01/simon-gilbert-autofac-azure-storage.png"
slug = "autofac-azure-table-storage-aspnetcore-3"
summary = "Keen to use Autofac's DI framework with Azure Table Storage? Simon Gilbert explains how to utilise Autofac with ASP.Net MVC Core 3.0"
title = "Using Autofac for Azure Table Storage with C# ASP.Net Core MVC 3.0"
tags = ["C#.Net", "Asp.Net MVC", "Dependency Injection", "Autofac", "Azure Table Storage"]
+++


### Azure Table Storage DI

Using Azure Table Storage is a very cost effective way to run a cloud hosted database in Microsoft Azure. The question is, how to we manage the DI.

### Enter "Autofac"

We've of course all heard of Autofac, so I won't explain the benefits of their framework.

{{< figure src="/images/2019/12/simon-gilbert-autofac-azure-storage-1.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}

### Implementation Steps

* Create our data model.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-2.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}

* Create our CRUD repository and inject an Azure CloudTable into the constructor.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-3.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



* Create our CRUD service layer and inject the repository into the constructor.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-4.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



* Implement a module to configure Azure Table Storage using Autofac's DI layer, in four steps -

1) Create an Azure Storage Account:

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-5a.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



2) Configure our service layer and our Azure Cloud Table:

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-5b.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



3) Configure our repository to use our Azure Cloud Table:

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-5c.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



* Configure Startup.cs file to use the Azure Table Storage Autofac module we've built.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-6.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



* Configure Program.cs file to use Autofac.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-7.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}

* Utilise our CRUD service at the controller level in ASP.Net MVC Core 3.0.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-8.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}



* View our Azure Table Storage NoSQL database to verify our implementation.

{{< figure src="/images/2020/01/simon-gilbert-autofac-azure-storage-result.png" caption="Autofac Azure Storage C# ASP.Net MVC Core 3.0" >}}

Enjoy!