# Azure Storage Service Broker

## Documentation

This demo was created from multiple documentation sources from Pivotal and Microsoft.  Refer to these sources for the most up to date details on the latest available version.

### Azure Service Broker

https://docs.pivotal.io/partners/azure-sb/index.html

### Azure SDK for Java

https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-storage?view=azure-java-stable#create-a-simple-spring-boot-application-with-the-spring-initializr

## Create and Bind Azure Service Broker

1. Create a JSON storage account parameters file named `storage-example-config.json`:

```
{
  "resourceGroup": "demoResourceGroup",
  "storageAccountName": "bademostorageaccount",
  "location": "westus",
  "accountType": "Standard_LRS"
}
```

1. Create an Azure Service Broker instance:

```
cf create-service azure-storage standard mystorage -c storage-example-config.json
```

2. Bind the service to your app:

```
cf bind-service myapp mystorage
```

## Create and Bind CUPS broker
