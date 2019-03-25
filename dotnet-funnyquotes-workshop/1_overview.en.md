# PCF .NET Demo
This solution demonstrates the use of multiple microservices built using multiple .NET technologies running on PCF. The primary goal is to show that PCF can handle legacy code that existed as early as .NET version 1.0, with common stacks introduced as part of .NET framework up to the newest .NET core application. The solution demonstrates a natural progression / evolution that a customer would take towards making their apps cloud native.

Technical Features Demonstrated:
- Deploying and running a .NET Core MVC application on the Linux stack.
- Run a RESTful API service using Entity Framework.
- Deploying and running a .NET Framework WebForms application on Windows stack.
- Run an ASMX service using traditional ADO.NET on Windows stack.
- Run a WCF service with Entity Framework on Windows stack.
- Bind to a MySQL database Marketplace service.
- OWIN bootstrap into IIS
- Service discovery with Eureka.
- Circuit breaker with Hystrix.
- Config server with Git repo.
- Overview of OAuth and SSO.

# Solution Projects
![Architecture](https://github.com/Pivotal-Field-Engineering/pace-workshop-content/blob/master/dotnet-funnyquotes-workshop/images/architecture.png)

The solution revolves around a simple application that displays random quotes when a button is pressed.
It also features a Kill command to simulate application failure.
* FunnyQuotesUIForms - Web forms GUI. Depends on Eureka and Config Server. Depending on config value served by Config Server, the source of the messages shown will be switched between local in memory, ASMX service, WCF service, or REST.
* FunnyQuotesLegacyService - Contains ASMX service and WCF service implementations for serving messages.
   * ASMX fetches it's data from database connectivity via classic DataSet / DataAdapter approach.
   * WCF featches data from database using a more modern approach using Entity Framework.
* FunnyQuotesServicesOwin - Rest based implementation using OWIN with WebAPI. Intended to be run on Windows stack with HWC.
* FunnyQuotesUICore - Modern .NET Core version running on Linux stack. Calls backend services directly from javascript.
* FunnyQuotesCookieDatabase - encapsulates the Entity Framework context.
* FunnyQuotesCommon - contains list of quotes for local use.

# How to build
* Copy the `manifest.yml` from the scripts folder into the first four projects in the list above.
* Update each file according to the requirements of its respective project. Optionally, these changes can be made in each lab.
* Ensure that the file is output on every build.
  * In Visual Studio, right-click the file and select Properties. Set Copy to Output Directory to Copy Always.
  
  * For non-Visual Studio users, open each .csproj file and search for the `'manifest.yml` file.
  
  ```
    <Content Include="manifest.yml" />
  ```
  
  Make the following modification to the Xml element.
  
  ```
    <Content Include="manifest.yml">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </Content>
  ```
  
* Publish each product using the supplied publish profile. The two options for publishing are as follows:
  * Open the solution in Visual Studio, right click each project and select Publish. 
  * For non-Visual Studio users, publish each project from the command line from the root directory of each project.
  
    For each of the three .NET Framework projects:
  
    ```
    
    ```
  
    For the one .NET Core project.
  
    ```
      dotnet publish -c Release /p:PublishProfile=Properties\PublishProfiles\FolderProfile.pubxml
    ```
  
* The compiled assemblies will be output to the `\publish\` folder.

### Prerequisites
* Docker with images for
  * Config server
  * Eureka
  * Hystrix dashboard
  * MySQL

**Source Repo:** https://github.com/Pivotal-Field-Engineering/funny-quotes-demo

**Config Repo:** https://github.com/Pivotal-Field-Engineering/funny-quotes-config
