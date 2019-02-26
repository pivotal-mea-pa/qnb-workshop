## Service Discovery

### Show service discovery locally
1. Pull down docker image

    ```
     > docker pull mayan31370/spring-eureka
    ```

1. Run it

    ```
     > docker run -d --name eureka -p 8761:8080/tcp mayan31370/spring-eureka
    ```

1. Launch FunnyQuotesLegacyService or FunnyQuotesServicesOwin from Visual Studio
1. Check the dashboard http://localhost:8761/ to confirm registration

### Service discovery on PCF
1. Provision eureka from PCF marketplace. Call instance "eureka"
1. Bind it to FunnyQuotesLegacyService & FunnyQuoteServicesOwin. Restart both
1. Navigate to service dashboard
  1. Apps Manager > Space > Services > Eureka > Manage
1. Confirm both apps are showing up in the registry. Explain how we can now resolve the URL endpoint by well known name

### Things to highlight in code
1. Open appsettings.json
  1. Explain `eureka:client` section when running locally
  1. This will be overriden when running on CloudFoundry
1. In startup.cs
  1. `services.AddDiscoveryClient(Configuration);` to register discovery services
  1. `app.UseDiscoveryClient();` to start using them
  1. `.AddHttpMessageHandler<DiscoveryHttpMessageHandler>();` when building up HttpClient factory
  1. Explain the use of HttpClientFactory (new in ASP.NET Core 2.1). Article explaining why it should be used instead of using HttpClient by hand https://www.stevejgordon.co.uk/introduction-to-httpclientfactory-aspnetcore