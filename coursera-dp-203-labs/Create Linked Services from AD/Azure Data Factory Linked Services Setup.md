```markdown
# Azure Data Factory Linked Services Setup

## Prerequisites
- Before executing the commands, make sure you have installed the Azure PowerShell module. If not, run the following command to install it:
  ```powershell
  Install-Module -Name Az -AllowClobber
  ```

## Command List

1. Connect to Azure:
   - Use either of the following commands to connect to Azure:
     ```powershell
     Add-AzureRmAccount
     # or
     Connect-AzAccount
     ```

2. Import the Azure PowerShell module:
   ```powershell
   Import-Module Az
   ```

3. Retrieve Azure subscriptions:
   ```powershell
   Get-AzSubscription
   ```

4. Set the desired subscription context:
   ```powershell
   Set-AzContext -SubscriptionId <subscriptionId>
   ```

5. Register the Azure Data Factory resource provider:
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.DataFactory
   ```

6. Create a new resource group (if needed):
   ```powershell
   $resourceGroupName = "myResourceGroup"
   $location = "West US 2"
   
   # Check if the resource group already exists
   Get-AzResourceGroup -Name $resourceGroupName
   
   # If the resource group doesn't exist, create it
   New-AzResourceGroup -Name $resourceGroupName -Location $location
   ```

7. Create a new Azure Data Factory:
   ```powershell
   - Execute the following command to create the Azure Blob Storage Linked Service:
   ```powershell
   $resourceGroupName = "<resource group name>"
   $resourceGroupName = "myResourceGroup"
   $location = "West US 2"
   
   # Create the Azure Data Factory
   New-AzDataFactoryV2 -ResourceGroupName $resourceGroupName -Name "myDataFactory" -Location $location
   ```

8. Create Azure SQL Database Linked Service:
   - Save the following JSON as a .json file on your local machine (e.g., `azuresql.json`):
   
   ```json
   {
     "name": "AzureSqlLinkedService",
     "properties": {
       "type": "AzureSqlDatabase",
       "typeProperties": {
         "connectionString": "Server=tcp:<server-name>.database.windows.net,1433;Database=ctosqldb;User ID=ctesta-oneill;Password=P@ssw0rd;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
       }
     }
   }
   ```
9. Create Azure Blob Storage Linked Service:
   - Save the following JSON as a .json file on your local machine (e.g., `storage.json`):
   
   ```json
   {
     "name": "StorageLinkedService",
     "properties": {
       "type": "AzureStorage",
       "typeProperties": {
         "connectionString": "DefaultEndpointsProtocol=https;AccountName=ctostorageaccount;AccountKey=<account-key>"
       }
     }
   }
   ```
   - Execute the following command to create the Azure SQL Database Linked Service:
   ```powershell
   $resourceGroupName = "<resource group name>"
   $dataFactoryName = "<data factory name>"
   
   Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName `
       -ResourceGroupName $resourceGroupName -Name "AzureSqlLinkedService" `
       -DefinitionFile ".\azuresql.json"
   ```

  

 $dataFactoryName = " <data factory name> "
   
   Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName `
       -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" `
       -DefinitionFile ".\storage.json"
   ```

Note: Make sure to replace the placeholder values (e.g., `<subscriptionId>`, `<server-name>`, `<database-name>`, `<username>`, `<password>`, `<resource group name>`, `<data factory name>`, `<account-key>`) with the appropriate values.

```

