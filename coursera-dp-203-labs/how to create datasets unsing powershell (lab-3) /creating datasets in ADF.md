

```md
# Azure Data Factory Dataset Creation Guide

This guide provides step-by-step instructions to create datasets in Azure Data Factory and configure them to reference Azure Blob storage. The datasets will include input and output files stored in a container within the Azure Storage account.

## Prerequisites

- Azure Storage account with a created container
- Azure Data Factory configured with the appropriate linked service for Azure Storage

## Steps

1. Create a container in your Azure Storage account:
   - Open your Azure Storage account and navigate to the "Containers" section.
   - Create a new container (e.g., "mycontainer") or use an existing one.
   - Make a note of the container name as you'll need it for the folder structure.

2. Upload the `input.txt` and `output.txt` files to the container:
   - Within the container, create two folders: "input" and "output".
   - Upload the `input.txt` file to the "input" folder.
   - Upload the `output.txt` file to the "output" folder.

   **input.txt**:
   ```
   This is the sample input file.
   You can replace this text with your actual input data.
   Make sure to save this file with the name input.txt.
   ```

   **output.txt**:
   ```
   This is the sample output file.
   You can replace this text with the desired output data.
   Make sure to save this file with the name output.txt.
   ```

3. Update the `input.json` file with the correct folder and file names:
   - Open the `input.json` file.
   - Modify the "folderPath" property in the "typeProperties" section to reflect the folder structure within the container (e.g., "mycontainer/input").
   - Ensure that the "fileName" property matches the name of the `input.txt` file.

   **input.json**:
   ```json
   {
       "name": "InputDataset",
       "properties": {
         "type": "AzureBlob",
         "linkedServiceName": {
           "referenceName": "AzureStorageLinkedService11",
           "type": "LinkedServiceReference"
         },
         "structure": {
           "type": "Binary"
         },
         "typeProperties": {
           "folderPath": "mycontainer/input",
           "fileName": "input.txt"
         }
       }
   }
   ```

4. Update the `output.json` file with the correct folder and file names:
   - Open the `output.json` file.
   - Modify the "folderPath" property in the "typeProperties" section to reflect the folder structure within the container (e.g., "mycontainer/output").
   - Ensure that the "fileName" property matches the name of the `output.txt` file.

   **output.json**:
   ```json
   {
       "name": "OutputDataset",
       "properties": {
         "type": "AzureBlob",
         "linkedServiceName": {
           "referenceName": "AzureStorageLinkedService11",
           "type": "LinkedServiceReference"
         },
         "structure": {
           "type": "Binary"
         },
         "typeProperties": {
           "folderPath": "mycontainer/output",
           "fileName": "output.txt"
         }
       }
   }
   ```

5. Set the Azure Data Factory name and resource group variables:
   ```powershell
   $dataFactoryName = "<your-data-factory-name>"
   $resourceGroupName = "<your-resource-group-name>"
   ```

6. Create the input dataset in Azure Data Factory:
   ```powershell


   Set-AzDataFactoryV2Dataset -DataFactoryName $dataFactoryName `
       -ResourceGroupName $resourceGroupName -Name "InputDataset" `
       -DefinitionFile ".\input.json"
   ```

7. Create the output dataset in Azure Data Factory:
   ```powershell
   Set-AzDataFactoryV2Dataset -DataFactoryName $dataFactoryName `
       -ResourceGroupName $resourceGroupName -Name "OutputDataset" `
       -DefinitionFile ".\output.json"
   ```

> Note: Make sure to replace `<your-data-factory-name>`, `<your-resource-group-name>`, and "mycontainer" with the appropriate values specific to your Azure environment.

By following these steps, you will be able to create the datasets in Azure Data Factory, upload the `input.txt` and `output.txt` files to the correct folder structure in your Azure Storage Blob container, and configure the dataset references to the linked service correctly.
```
```

