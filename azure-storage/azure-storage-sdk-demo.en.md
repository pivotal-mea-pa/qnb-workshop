# Using Azure Storage SDK from Spring Boot

## Configure Azure Storage Starter

1. Edit your project `pom.xml` file to include the Azure Storage:

```
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>spring-azure-starter-storage</artifactId>
   <version>1.0.0.M2</version>
</dependency>
```

2. Save and close the `pom.xml` file.

## Create Azure Credentials Files

1. Open a terminal window and change directory to your project's resource folder:

```
$ cd /Users/barryalexander/demos/azure-storage-demo/src/main/resources
```

1. Log into your Azure subscription:

```
az login
```
2. List subscriptions

```
az account list
```
3. Copy the GUID value for the id attribute in the resultant JSON output from the `az account list` command.  This is the subscription id.

4. Set the account subscription id:

```
az account set -s <subscription id>
```
5. Create an Azure Credential File:

```
az ad sp create-for-rbac --sdk-auth > my.azureauth
```
This command will produce a JSON file in the resources folder.

## Configure Spring Boot to use the Azure Storage account.

1. Edit the `application.properties` file in the resources folder:

```
spring.cloud.azure.credential-file-path=my.azureauth
spring.cloud.azure.resource-group=demoResourceGroup
spring.cloud.azure.region=westus
spring.cloud.azure.storage.account=bademostorageaccount
```
## Sample code for basic Azure storage functionality

1. Ensure the AzureStorageDemoApplication java source looks like this:

```
package io.pivotal.azurestoragedemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AzureStorageDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(AzureStorageDemoApplication.class, args);
	}

}
```
2. Add a web controller class

```
package io.pivotal.azurestoragedemo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;
import org.springframework.core.io.WritableResource;
import org.springframework.util.StreamUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;
import java.io.OutputStream;
import java.nio.charset.Charset;

@RestController
public class WebController {

    @Value("blob://test/myfile.txt")
    private Resource blobFile;

    @GetMapping(value = "/")
    public String readBlobFile() throws IOException {
        return StreamUtils.copyToString(
                this.blobFile.getInputStream(),
                Charset.defaultCharset()) + "\n";
    }

    @PostMapping(value = "/")
    public String writeBlobFile(@RequestBody String data) throws IOException {
        try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
        }
        return "File was updated.\n";
    }
}
```
Note ``@Value("blob://[container]/[blob]")`` syntax respectively defines the names of the container and blob where you want to store the data

3. Change directory to the project root where the the `pom.xml` file is located

```
pwd
/Users/barryalexander/demos/azure-storage-demo
ls
HELP.md			mvnw			pom.xml	mvnw.cmd		src
```

4. Build the app and run locally:

```
.\mvnw clean package
.\mvnw spring-boot:run
```

5. Test using `POST` and `GET`:

```
curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
File was updated.
curl -X GET http://localhost:8080/
Hello World
```

6. Proceed to the next section to use the Azure Service Broker to use Azure Storage
