## Service Discovery

### Prerequisites
1. Ensure only the following services are defined in the manifest.yml file of the FunnyQuotesLegacyService and/or FunnyQuotesServicesOwin project.

    ```
    services:
        - mysql-funnyquotes
        - config-server
        - eureka
        #- hystrix
    ```

### Service discovery locally
1. Pull down docker image.

    ```
     > docker pull mayan31370/spring-eureka
    ```

1. Run it.

    ```
     > docker run -d --name eureka -p 8761:8080/tcp mayan31370/spring-eureka
    ```

1. Launch FunnyQuotesLegacyService or FunnyQuotesServicesOwin from Visual Studio.
1. Check the dashboard http://localhost:8761/ to confirm registration.
1. For FunnyQuotesLegacyService, append /FunnyQuoteServiceLegacy.asmx/GetCookie to the url to observe the result.
1. For FunnyQuotesServicesOwin, append /funnyquotes/random to the url to observe the result.

### Service discovery on PCF
1. Provision the p-service-registry service, the standard plan, and name it `eureka`.

    ```
        cf create-service p-service-registry standard eureka
    ```
1. Uncomment the eureka service in the manifest.yml file of both FunnyQuotesLegacyService & FunnyQuoteServicesOwin projects.
1. Push both applications.

    ```
        cd publish\FunnyQuotesLegacyService
        cf push
    ```
    ```
        cd publish\FunnyQuoteServicesOwin
        cf push
    ```
    
1. Navigate to service dashboard.
    Apps Manager > Space > Services > Eureka > Manage.
    
1. Confirm both apps are showing up in the registry. 

Note that URL endpoints are discovered by a well-known name.

### Observations to highlight in code
1. Open appsettings.json.
  1. Note `eureka:client` section when running locally.
  1. This will be overriden when running on CloudFoundry.
1. Open Startup.cs
  1. The line, `services.AddDiscoveryClient(Configuration);` is used to register services to be discovered.
  1. The line, `.AddHttpMessageHandler<DiscoveryHttpMessageHandler>();` is to build up the HttpClient factory.
  1. The line, `app.UseDiscoveryClient();` makes discovery ready for use.
  1. Note the use of HttpClientFactory (new in ASP.NET Core 2.1). Article explaining why it should be used instead of using HttpClient by hand https://www.stevejgordon.co.uk/introduction-to-httpclientfactory-aspnetcore.
