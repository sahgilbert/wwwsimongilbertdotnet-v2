+++
author = "Simon Gilbert"
categories = [ "C#", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "Dev", "Dot Net", "Github", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Object Oriented Programming", "Programming", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Microsoft Azure", "Azure", "Azure App Service", "Azure Table Storage", "Dependency Injection", "Microsoft Identity", "Microsoft Entra ID"]
date = 2024-02-07T10:38:00Z
description = ""
draft = false
image = "/images/2024/03/simon-gilbert-dev-blog-landscape-2024-3.png"
slug = "dependency-injection-autofac-azure-table-storage-microsoft-identity"
summary = "Keen to improve the security of your Azure Table Storage Access? Today I'll explain how to ensure only your dedicated Azure App Service has access to the database when running in production - No more Access Keys or Shared Access Signatures required!"
title = "Dependency Injection for Azure Table Storage including Microsoft Entra Identity Security using C#.Net 8.0.0 and Autofac"
tags = ["C#.Net", "Asp.Net MVC", "Azure Table Storage", "Autofac", "Dependency Injection"]
+++


### Autofac
Autofac simplifies dependency injection in C#.NET by providing a flexible and extensible framework. With its intuitive syntax, Autofac efficiently manages object lifetimes and resolves dependencies, promoting clean and modular code design. This powerful tool enhances maintainability and testability in software development projects.

### Autofac with Azure Table Storage
Leveraging Autofac for Azure Table Storage involves integrating the Azure.Data.Tables NuGet package seamlessly. By configuring Autofac to manage dependencies, developers can inject Azure Table Storage-related services and repositories effortlessly. This approach promotes decoupling and facilitates the unit testing of components interacting with Azure Table Storage, ensuring a scalable and maintainable solution in C#.NET applications.

### One Step Further...Microsoft Entra Identity
Microsoft Entra Identity enhances security for Azure Table Storage by providing robust identity and access management. Leveraging Entra Identity ensures secure and authenticated interactions with Azure Table Storage resources. This identity solution integrates seamlessly, offering features like multi-factor authentication and role-based access control. By incorporating Entra Identity into the architecture, developers can fortify their Azure Table Storage implementations with advanced security measures, safeguarding sensitive data and mitigating potential risks effectively.

Today, we're going to build a service layer that integrates with Microsoft Azure Table Storage, combined with Microsoft Entra Identity, with all the dependency injection being handled by Autofac...

### Nuget Package Installation
First, let's install some Nuget packages:

{{< figure src="/images/2024/03/simon-gilbert-dev-blog-dependency-injection-autofac-1.png" caption="Visual Studio Nuget Packages for Dependency Injection" >}}

### Service Class
Now we can create our Service Class:

```C#
public sealed class DemoService : IDemoService
{
    private readonly IDemoRepository _demoRepository;

    public DemoService(IDemoRepository demoRepository)
    {
        _demoRepository = demoRepository;
    }

    public async Task<AzureTableStorageResponse> Create()
    {
        var newsletterSubscription = new NewsletterSubscription
        {
            PartitionKey = Guid.NewGuid().ToString(),
            RowKey = Guid.NewGuid().ToString(),
            EmailAddress = "hello@test.com",
            CreatedDateTimeUtc = DateTime.UtcNow,
            LastUpdatedDateTimeUtc = DateTime.UtcNow,
        };

        var azureTableStorageResponse = await _demoRepository.Create(newsletterSubscription);

        return azureTableStorageResponse;
    }

    public async Task<AzureTableStorageResponse> Update()
    {
        var partitionKey = "Simon";
        var rowKey = "11912ddc-2709-478c-a5d0-a8756b56f6e8";
        var existingEntity = await _demoRepository.Get(partitionKey, rowKey);

        existingEntity.EmailAddress = $"Email is now {DateTime.UtcNow}";

        var azureTableStorageResponse = await _demoRepository.Update(existingEntity);

        return azureTableStorageResponse;
    }

    public async Task<NewsletterSubscription> Get(string partitionKey, string rowKey)
    {
        var result = await _demoRepository.Get(partitionKey, rowKey);

        return result;
    }

    public async Task<HashSet<NewsletterSubscription>> GetAll(string partitionKey)
    {
        var results = await _demoRepository.GetAll(partitionKey);

        return results;
    }

    public async Task<HashSet<NewsletterSubscription>> GetAll()
    {
        var results = await _demoRepository.GetAll();

        return results;
    }
}
```

### Repository Class
Now let's create our Repository Class, which has a private readonly instance of the Azure Table Storage TableClient:

```C#
public class DemoRepository : IDemoRepository
	{
        private readonly TableClient _tableClient;

        public DemoRepository(TableClient tableClient)
        {
            _tableClient = tableClient;
        }

        public async Task<AzureTableStorageResponse> Create(NewsletterSubscription newsletterSubscription)
        {
            string storageException = string.Empty;
            string additionalException = string.Empty;

            try
            {
                Response response = await _tableClient.AddEntityAsync(newsletterSubscription);

                var azureTableStorageResponse = response.HandleAzureResponse();

                return azureTableStorageResponse;
            }
            catch (TableTransactionFailedException ex)
            {
                storageException = ex.HandleTableTransactionFailedException();
            }
            catch (Exception ex)
            {
                additionalException = ex.HandleAdditionalStorageException();
            }
            
            return new AzureTableStorageResponse()
            {
                WasSuccessful = false,
                StorageException = storageException,
                AdditionalStorageException = additionalException
            };
        }

        public async Task<AzureTableStorageResponse> Update(NewsletterSubscription newsletterSubscription)
        {
            string storageException = string.Empty;
            string additionalException = string.Empty;

            try
            {
                
                //Response response = await _tableClient.UpdateEntityAsync(newsletterSubscription, newsletterSubscription.ETag);
                Response response = await _tableClient.UpsertEntityAsync(newsletterSubscription);

                var azureTableStorageResponse = response.HandleAzureResponse();

                return azureTableStorageResponse;
            }
            catch (TableTransactionFailedException ex)
            {
                storageException = ex.HandleTableTransactionFailedException();
            }
            catch (Exception ex)
            {
                additionalException = ex.HandleAdditionalStorageException();
            }

            return new AzureTableStorageResponse()
            {
                WasSuccessful = false,
                StorageException = storageException,
                AdditionalStorageException = additionalException
            };
        }

        public async Task<NewsletterSubscription> Get(string partitionKey, string rowKey)
        {
            var existingEntity = await _tableClient
                .GetEntityAsync<NewsletterSubscription>(partitionKey, rowKey);

            return existingEntity.Value;
        }

        public async Task<HashSet<NewsletterSubscription>> GetAll(string partitionKey)
        {
            AsyncPageable<NewsletterSubscription> queryResult = _tableClient
                .QueryAsync<NewsletterSubscription>(x => x.PartitionKey == partitionKey);

            var hashSet = new HashSet<NewsletterSubscription>();

            await foreach (var entity in queryResult)
            {
                hashSet.Add(entity);
            }

            return hashSet;
        }

        public async Task<HashSet<NewsletterSubscription>> GetAll()
        {
            AsyncPageable<NewsletterSubscription> queryResult = _tableClient
                .QueryAsync<NewsletterSubscription>();

            var hashSet = new HashSet<NewsletterSubscription>();

            await foreach (var entity in queryResult)
            {
                hashSet.Add(entity);
            }

            return hashSet;
        }
    }
```

### Autofac Module
Now let's create a Module, that derives from the "Module" class in the Autofac library:

```C#
public sealed class AutofacAzureTableStorageModule : Module
{
    protected override void Load(ContainerBuilder containerBuilder)
    {
        ConfigureAzureStorageTableServiceClient(containerBuilder);
        ConfigureAzureStorageTableClients(containerBuilder);
        RegisterDefaultAzureCredential(containerBuilder);
        ConfigureServicesWithRepositories(containerBuilder);
        ConfigureRepositories(containerBuilder);
    }

    private void ConfigureAzureStorageTableServiceClient(ContainerBuilder containerBuilder)
    {
        containerBuilder.Register(c => CreateAzureTableServiceClient(c.Resolve<DefaultAzureCredential>()))
            .As<TableServiceClient>()
            .SingleInstance();
    }

    private TableServiceClient CreateAzureTableServiceClient(DefaultAzureCredential credential)
    {
        // Specify the Azure Storage Account name
        const string AZURE_STORAGE_ACCOUNT_NAME = "[INSERT_AZURE_STORAGE_ACCOUNT_NAME]";
        const string AZURE_STORAGE_ACCOUNT_URL = $"https://{AZURE_STORAGE_ACCOUNT_NAME}.table.core.windows.net";

        var uri = new Uri(AZURE_STORAGE_ACCOUNT_URL);

        return new TableServiceClient(uri, credential);
    }

    private void ConfigureAzureStorageTableClients(ContainerBuilder containerBuilder)
    {
        containerBuilder.Register(c => GetTableClient(c.Resolve<TableServiceClient>(),
            AzureTableStorageCloudTableConfig.DEMO_AZURE_TABLE_STORAGE_CLOUD_TABLE_NAME))
            .As<TableClient>()
            .InstancePerLifetimeScope();
    }

    private TableClient GetTableClient(TableServiceClient tableServiceClient, string tableName)
    {
        var tableClient = tableServiceClient.GetTableClient(tableName);

        tableClient.CreateIfNotExists();

        return tableClient;
    }

    private void RegisterDefaultAzureCredential(ContainerBuilder containerBuilder)
    {
        containerBuilder.Register(c => new DefaultAzureCredential())
                .AsSelf()
                .InstancePerLifetimeScope();
    }

    private void ConfigureServicesWithRepositories(ContainerBuilder containerBuilder)
    {
        containerBuilder.RegisterType<DemoService>()
            .AsImplementedInterfaces()
            .InstancePerLifetimeScope();
    }

    private void ConfigureRepositories(ContainerBuilder containerBuilder)
    {
        containerBuilder.RegisterType<DemoRepository>()
            .As<IDemoRepository>()
            .InstancePerLifetimeScope();
    }
}
```

### Finally
[Follow the steps in this link to configure Microsoft Entra Identity for Azure Table Storage/App Service](https://learn.microsoft.com/en-us/dotnet/azure/sdk/authentication/azure-hosted-apps?tabs=azure-portal%2Cazure-app-service%2Ccommand-line)

Enjoy!