{{ $flowhome := .Site.GetPage "/workshop" }}
### Development Tools
[SDK for .NET Core and dotnet CLI](../demos/sdk-for-.net-core-and-dotnet-cli)  
[IDEs](../demos/ides)  
{{ if in $flowhome.Title "Concourse" }}
[Deploy Concourse](../demos/deploy-concourse)  
{{ end }}
[CF Dev](../demos/cf-dev)

### .NET Core App
[Setting Up the App](../demos/setting-up-the-app) 
[Setting Up xUnit](../demos/setting-up-xunit)  
[Creating a RESTful API](../demos/creating-a-restful-api)  
[Introducing Fluent Assertions](../demos/introducing-fluent-assertions)  
[Making the App Asynchronous](../demos/making-the-app-asynchronous)  
[Delegating Data Responsibility](../demos/delegating-data-responsibility)  
[Introducing an In-Memory Database](../demos/introducing-an-in-memory-database)  
[Adding Other Endpoints](../demos/adding-other-endpoints)  
[Adding Integration Tests](../demos/adding-integration-tests)  
[Adding a Persistence Layer](../demos/adding-a-persistence-layer)

### Continuous Integration / Deployment
{{ if in $flowhome.Title "Concourse" }}
[CI Setup](../demos/ci-setup)  
{{ end }}
[CF Deployment](../demos/cf-deployment)  
[Connect to a DB on CF](../demos/connect-to-a-db-on-cf)  
[Smoke Tests](../demos/smoke-tests)  
[Zero-Downtime Deployment](../demos/zero-downtime-deployment)
