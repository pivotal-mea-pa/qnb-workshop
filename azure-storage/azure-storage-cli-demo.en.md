# Azure Storage

In this section we will use the Azure CLI to create and test Azure Blob storage.

## Azure CLI steps

1. Create a resource group:
```
az group create \
    --name demoResourceGroup \
    --location westus
```
2. Create storage account:
```
az storage account create \
    --name bademostorageaccount \
    --resource-group demoResourceGroup \
    --location westus \
    --sku Standard_LRS \
    --encryption blob
```
## Specify storage account credentials

We will get the storage account and key and store them in environment variables to test access to blob Storage

1. List storage account keys:
```
az storage account keys list \
    --account-name bademostorageaccount \
    --resource-group demoResourceGroup \
    --output table
```
2. Set `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_KEY` environment variables
```
export AZURE_STORAGE_ACCOUNT="bademostorageaccountname"
export AZURE_STORAGE_KEY="<storage account key"
```
## Create a container

```
az storage container create --name bademostoragecontainer
```
## Create and upload a file
```
echo "Hello World" >> helloworld
az storage blob upload \
    --container-name bademostoragecontainer \
    --name helloworld \
    --file helloworld
```
You should see something like this returned:
```
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D6C6A9DC596CC1\"",
  "lastModified": "2019-04-21T22:36:56+00:00"
}
```
## Download the blob to a file
```
az storage blob download \
    --container-name bademostoragecontainer \
    --name helloworld \
    --file ~/Downloads/helloworld.txt
cat ~/Downloads/helloworld.txt
Hello World
```

[Up to date instructions are outlined here](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli) to create the blob storage used in this demo.
