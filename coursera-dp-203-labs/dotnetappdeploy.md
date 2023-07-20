
## Deploying the dotnet app on azure 
```markdown
# Deploying a .NET App in Azure

## Prerequisites
- Make sure you have the Azure CLI (`az`) installed.

## Steps

1. Run the following commands in PowerShell:
```powershell
az account show
az login
```

2. Create a storage account by executing the following command:
```powershell
az storage account create \
  --kind StorageV2 \
  --resource-group [sandbox resource group name] \
  --location centralus \
  --name [your-unique-storage-account-name]
```

3. Clone and explore the unfinished app:
```powershell
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git 
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

4. Install the required packages to configure and run the app:
```powershell
dotnet nuget add source --name nuget.org https://api.nuget.org/v3/index.json
dotnet add package WindowsAzure.Storage
dotnet restore
```

5. Modify the `BlobStorage.cs` file with the following code:
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Extensions.Options;

using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace FileUploader.Models
{
    public class BlobStorage : IStorage
    {
        private readonly AzureStorageConfig storageConfig;

        public BlobStorage(IOptions<AzureStorageConfig> storageConfig)
        {
            this.storageConfig = storageConfig.Value;
        }

        // Add/modify the necessary methods for creating, deleting, listing, etc., in the BlobStorage class.
    }
}
```

6. Add the connection string in the `appsettings.json` file. Use the following command to retrieve the connection string:
```powershell
az storage account show-connection-string --name testazstorage1999 --output json
```
The `appsettings.json` file should look like this:
```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "ConnectionStrings": {
    "StorageConnectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=testazstorage1999;AccountKey=0ubBwbg+BTKPbxULt8F9R30EO58rSA5V7/zwFvSAoP1L863YevMUBgGMEVOHB7H40JMdLBtsBcEI+ASt0wcnwg==;BlobEndpoint=https://testazstorage1999.blob.core.windows.net/;FileEndpoint=https://testazstorage1999.file.core.windows.net/;QueueEndpoint=https://testazstorage1999.queue.core.windows.net/;TableEndpoint=https://testazstorage1999.table.core.windows.net/"
  }
}
```

## Deploying

Here are the final commands for deploying and running a .NET app in Azure:

1. Create an App Service plan:
```powershell
az appservice plan create --name blob-exercise-plan --resource-group [sandbox resource group name] --sku FREE --location eastus
```

2. Create a Web App:
```powershell
az webapp create --name <your-unique-app-name> --plan blob-exercise-plan --resource-group [sandbox resource group name]
```
Replace `<your-unique-app-name>` with your desired app name and `[sandbox resource group name]` with the name of your

 sandbox resource group.

3. Configure the Web App:
```powershell
$CONNECTIONSTRING = az storage account show-connection-string --name <your-unique-storage-account-name> --output tsv
az webapp config appsettings set --name <your-unique-app-name> --resource-group [sandbox resource group name] --settings AzureStorageConfig:ConnectionString="$CONNECTIONSTRING" AzureStorageConfig:FileContainerName=files
```
Replace `<your-unique-app-name>` with your app name and `[sandbox resource group name]` with the name of your sandbox resource group.

4. Deploy your app:
```powershell
dotnet publish -o pub
cd pub
Compress-Archive -Path * -DestinationPath ../site.zip
az webapp deployment source config-zip --src ../site.zip --name <your-unique-app-name> --resource-group [sandbox resource group name]
```
Replace `<your-unique-app-name>` with your app name and `[sandbox resource group name]` with the name of your sandbox resource group.

5. View and test your app:
- Open a web browser and visit `https://<your-unique-app-name>.azurewebsites.net` to see your deployed app.
- To view the uploaded blobs in the container, run the following command:
```powershell
az storage blob list --account-name <your-unique-storage-account-name> --container-name files --query "[].name" --output table
```
Replace `<your-unique-storage-account-name>` with your storage account name.

## Running the App Locally
If you want to run the app locally:

1. Make the following change in the `Program.cs` file:
```csharp
webBuilder.UseUrls("http://localhost:5001"); // Change the port number here
```

2. Run the following commands:
```powershell
dotnet build
dotnet run
```

Remember to replace placeholders like `<your-unique-app-name>`, `[sandbox resource group name]`, and `<your-unique-storage-account-name>` with appropriate values based on your setup.

These commands should help you successfully deploy and run your .NET app in Azure. If you encounter any issues or have further questions, feel free to ask.
```

Please note that the commands and instructions in the notes assume you have the necessary prerequisites and have made the required configurations specific to your environment. Feel free to modify the instructions as needed.

Let me know if there's anything else I can assist you with!
