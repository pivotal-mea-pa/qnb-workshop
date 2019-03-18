# PCF .NET Demo
This solution demonstrates the use of multiple microservices built using multiple .NET technologies running on PCF. The primary goal is to show that PCF can handle legacy code that existed as early as .NET version 1.0, with common stacks introduced as part of .NET framework up to the newest .NET core application. The solution demonstrates a natural progression / evolution that a customer would take towards making their apps cloud native.

Technical Features Demonstrated:
- Deploying and running a .NET Core MVC application on the Linux stack.
- Run a RESTful API service using Entity Framework.
- Deploying and running a .NET Framework WebForms application on Windows stack.
- Run an ASMX service using traditional ADO.NET on Windows stack.
- Run a WCF service with Entity Framework on Windows stack.
- OWIN bootstrap into IIS
- Service discovery with Eureka.
- Circuit breaker with Hystrix.
- Config server with Git repo.
- Bind to a MySQL database Marketplace service.

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
* Open Visual Studio. 
* Publish each product using supplied publish profile.
* Compiled assemblies are output to `\publish\` folder.
* Copy `manifest.yml` file into each publish folder, making necessary changes per respective apps.
* For each lab, there are two options for creating services: 1) Run `create-services.bat` in `\scripts` folder to create all marketplace services needed for all labs, or 2) Create same services as you progress through each lab that requires their use.
* For each lab, there are two options for pushing the code: 1) Push each app from each publish folder, or 2) Only push the apps as you progress through each lab.

NOTE: If there is a need to build and publish more than once, it is easier to add the manifest file to each project and make required changes per app. On the Properties tab, set Copy to Output Directory to Copy Always. This will add the manifest every time the app is built and published.

### Prerequisites
* Visual Studio 2017 with .NET core support
* Docker with images for
  * Config server
  * Eureka
  * Hystrix dashboard
  * MySQL

**Source Repo:** https://github.com/Pivotal-Field-Engineering/funny-quotes-demo

**Config Repo:** https://github.com/Pivotal-Field-Engineering/funny-quotes-config
