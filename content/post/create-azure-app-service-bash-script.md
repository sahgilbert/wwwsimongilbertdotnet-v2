+++
author = "Simon Gilbert"
categories = [ "C#", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "Dev", "Dot Net", "Github", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Object Oriented Programming", "Programming", "REST", "REST Api", "RESTful", "RESTful Api", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Microsoft Azure", "Azure", "Azure App Service", "Bash Script", "Scripting Language"]
date = 2024-01-01T10:38:00Z
description = ""
draft = false
image = "/images/2024/01/simon-gilbert-dev-blog-landscape-2024-1.png"
slug = "create-azure-app-service-bash-script"
summary = "Looking to setup an Azure App Service, but tired of configuring it all in the Azure Portal Dashboard? There's a solution to that...In today's blog I'm going to show you a quick way of setting up your managed service via the Azure-CLI, using a Bash Script."
title = "Creating an Azure App Service via Azure-CLI using a Bash Script"
tags = ["Azure", "Azure Web App Service", "Azure-CLI", "Bash Script"]
+++


### Microsoft Azure - App Services

Microsoft Azure App Services streamlines web application development, deployment, and scaling in the cloud. Supporting diverse programming languages and frameworks, it offers features for web apps, mobile app backends, and RESTful APIs. With auto-scaling and continuous deployment, Azure App Services ensures high availability, scalability, and seamless integration within the Azure ecosystem, making it an ideal choice for efficient application hosting and management.

### Managed Services vs. Server Infrastructure
Managed services eliminate infrastructure management complexities, relieving developers from tasks like patching and scaling. This abstraction enhances productivity, promotes cost efficiency, and accelerates time-to-market, allowing organizations to focus on application development without the burden of manual configuration.

### The Benefits of Bash Scripts
Creating an Azure App Service using a Bash script streamlines deployment by automating the process. With the script, developers can define configurations, set parameters, and deploy the app without the need for manual steps through the Azure portal dashboard. This not only ensures consistency across deployments but also enables version control of deployment configurations. The script-driven approach enhances reproducibility, making it easier to scale applications and maintain a standardized deployment process. Additionally, developers can integrate this script into continuous integration/continuous deployment (CI/CD) pipelines for a more efficient and automated software delivery lifecycle.

Here's the Bash Script...

```Bash
#!/bin/bash

# Azure configuration
resourceGroupName="simons-example-resource-group"
appServiceName="simons-example-app-service"
location="UK South"
servicePlanSku="F1"  # Free pricing tier

# Log in to Azure
az login

# Check if the resource group already exists
if az group show --name $resourceGroupName --query "name" --output tsv 2>/dev/null; then
    echo "Resource group '$resourceGroupName' already exists."
else
    # Create the resource group
    az group create --name $resourceGroupName --location "$location"
    echo "Resource group '$resourceGroupName' created."
fi

# Check if the service plan already exists
if az appservice plan show --name $appServiceName-Plan --resource-group $resourceGroupName --query "name" --output tsv 2>/dev/null; then
    echo "App Service Plan '$appServiceName-Plan' already exists."
else
    # Create the App Service Plan
    az appservice plan create --name $appServiceName-Plan --resource-group $resourceGroupName --sku $servicePlanSku
    echo "App Service Plan '$appServiceName-Plan' created."
fi

# Create the App Service
az webapp create --name $appServiceName --resource-group $resourceGroupName --plan $appServiceName-Plan

echo "App Service '$appServiceName' created in resource group '$resourceGroupName' in location '$location'."
```

...Now before we run it, we need to execute a quick command in the terminal:

```Terminal
chmod +x create-azure-app-service.sh
```

The chmod +x create-azure-app-service.sh command is used to add the execute permission to a file in Unix-like operating systems, including Linux and macOS. In this case, it's applied to a script file named create-azure-app-service.sh. Let's break down the command:

- chmod: Stands for "change mode," and it's a command used to change the file permissions.
- +x: Adds the execute permission to the file.
- create-azure-app-service.sh: The name of the file to which the permissions are being changed.

After running this command, the file create-azure-app-service.sh becomes executable, meaning you can run it as a program or script directly from the command line. The command is a way to make sure that the script can be executed by the user. Once you've set the execute permission, you can run the script using ./create-azure-app-service.sh in the terminal.

...and just like that, your Microsoft Azure App Service is created in Azure, ready for you to deploy some awesome code to!

{{< figure src="/images/2024/01/simon-gilbert-dev-blog-azure-app-service-bash-script-result.png" caption="Azure-CLI App Service Bash Script Result" >}}

{{< figure src="/images/2024/01/simon-gilbert-dev-blog-azure-app-service-portal-result.png" caption="Azure Portal App Service Result" >}}

Enjoy!