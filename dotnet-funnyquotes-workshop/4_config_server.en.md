## Config Server

We want to get FunnyQuotes client to use a different implementation of IFunnyQuotesService to talk to an alternative backend service. There are multiple named implementations registered into dependency injection container. The instance supplied for IFunnyQuotesService is dependent on the config value. We're going to use config settings to swap out the implementation it will use.

### Prerequisites
1. Ensure only the following services are defined in the manifest.yml files of all four published applications.

    ```
    services:
        - mysql-funnyquotes
        - config-server
        #- eureka
        #- hystrix
    ```

### Explaining config server locally
1. Create a docker instance of config server:

    ```
     > docker pull hyness/spring-cloud-config-server
     > docker run --name configserver -p 8888:8888 -d -e SPRING_CLOUD_CONFIG_SERVER_GIT_URI=https://github.com/Pivotal-Field-Engineering/funny-quotes-config hyness/spring-cloud-config-server
    ```
    
1. Observe config server serving up values for different apps and profiles:
    1. http://localhost:8888/FunnyQuotesUIForms/Production
    1. http://localhost:8888/FunnyQuotesUICore/Production
    1. http://localhost:8888/FunnyQuotesUICore/Development

1. Note the usage of environment (second part of URL) to override settings. In .NET Core apps this value comes from `ASPNETCORE_ENVIRONMENT` environmental variable.

### On PCF
1. Fork the funny-quotes-config repo from https://github.com/Pivotal-Field-Engineering/funny-quotes-config.
1. Edit gitconfig.json in scripts folder and update URL to forked repo.
1. Provision p-config-server from the marketplace, the standard plan, and name it `config-server` from the scripts folder.
    
    ```
    > cf create-service p-config-server standard config-server -c gitconfig.json
    ```
    
1. Push all four applications.
1. Launch FunnyQuotesUICore and FunnyQuotesUIForms and note the provider in both.
1. In the forked repo, change ClientType from rest to local in the FunnyQuotesUICore.yaml file.

  ```
    FunnyQuotes:
      ClientType: local
      FailedMessage: Failure is not an option -- it comes bundled with Windows.
  ```
  
  Save settings. Run git commit and push. After 10 seconds, refresh FunnyQuotesUICore.
  Note the new provider is changed without a restart of the app.

1. In the forked repo, change ClientType from wcf to asmx in the FunnyQuotesUICore.yaml file.

  ```
    FunnyQuotes:
      ClientType: asmx
  ```
    
  Save settings. Run git commit and push. After 10 seconds, refresh FunnyQuotesUIForms.
  Note the new provider is changed without a restart of the app.

  Feel free to experiment with the other client types in FunnyQuotesUIForms.

  Possible values include:
    - local, uses memory.
    - rest, uses FunnyQuotesServicesOwin.
    - wcf, uses FunnyQuotesLegacyService.
    - asmx, uses FunnyQuotesLegacyService.

### Things to note

* Integrated security of config server secured by OAuth. See VCAP_Services to highlight this fact.
* Config server is a Java Spring app.
* Spring uses the same layered config as in .NET core. Environmental variable to set URL of repo is actually using the same config sementics but in Java.

### Things to highlight in code

1.  `.AddConfigServer(env, logFactory)` in Global.cs (legacy apps) or Program.cs (.NET core). 
1. Open up appsettings.json.
    1. Note that name of app comes from `spring:application:name`.
    1. Note that `spring:cloud:config` section determines settings when running config server locally.
1. Different packages `Pivotal` vs `Steeltoe`. Difference is OAuth2 security. Pivotal package will work with non secured as well.
1. Using Pivotal package will also add Cloud Foundry VCAP variables as a config source.
1. Observe registration and selection of DI instances based on config value in FunnyQuotesUIForms.Global.asmx.cs:

    ```csharp
      builder.RegisterType<AsmxFunnyQuotesClient>().Named<IFunnyQuoteService>("asmx");
      builder.RegisterType<WcfFunnyQuotesClient>().Named<IFunnyQuoteService>("wcf");
      builder.RegisterType<LocalFunnyQuoteService>().Named<IFunnyQuoteService>("local");
      builder.RegisterType<RestFunnyQuotesClient>().amed<IFunnyQuoteService>("rest");
      // register dynamic resolution of implementation of IFunnyQuoteService based on named implementation defined in the config
      builder.Register(c =>
      {
          var quotesConfig = c.Resolve<IOptionsSnapshot<FunnyQuotesConfiguration>>();
          return c.ResolveNamed<IFunnyQuoteService>(quotesConfig.Value.ClientType);
      })
    ```                
                
### Feature toggles
1. We can push disabled code into prod and using config to activate it vs branching.
1. Observe usage of it in codebase for security. Don't enable yet:
    1. Startup.cs in FunnyQuotesUICore

    ```csharp
         if (funnyquotesConfig.EnableSecurity) 
           authBuilder.AddCloudFoundryOAuth(Configuration);
    ```     
    
    1. Startup.cs in FunnyQuotesServicesOwin.
    
    ```csharp
        if(funnyQuotesConfig.EnableSecurity)
          app.UseCloudFoundryJwtBearerAuthenticationconfig); 
        else
          app.Use<NoAuthenticationMiddleware>(); 
    ```
