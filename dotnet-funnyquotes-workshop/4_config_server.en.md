## Config Server

We want to get FunnyQuotes client to use a different implementation of IFunnyQuotesService to talk to backend instead of pulling quotes from memory. There are multiple named implementations registered into dependency injection container. The instance supplied for IFunnyQuotesService is dependent on config value. We're going to use config settings to swap out the implementation it will use 

### Explaining config server locally
1. Create a docker instance of config server:
    ```
     > docker pull hyness/spring-cloud-config-server
     > docker run --name configserver -p 8888:8888 -d -e SPRING_CLOUD_CONFIG_SERVER_GIT_URI=https://github.com/Pivotal-Field-Engineering/funny-quotes-config hyness/spring-cloud-config-server
    ```
    
1. Show how config server serving up values for different apps and profiles:
    1. http://localhost:8888/FunnyQuotesUIForms/Production
    1. http://localhost:8888/FunnyQuotesUICore/Production
    1. http://localhost:8888/FunnyQuotesUICore/Development

1. Explain the usage of environment (second part of URL) to override settings. In .NET Core apps this value comes from `ASPNETCORE_ENVIRONMENT` environmental variable

### On PCF
1. Fork funny-quotes-config (so you can push to it)
1. Edit gitconfig.json (in root folder) and adjust URL to forked repo
1. Provision config server from marketplace
    
    ```
    > cf create-service p-config-server standard config-server -c gitconfig.json
    ```
    
1. Bind to all 4 applications and restart all apps
1. Open FunnyQuotesUICore and highlight the different provider being used
1. Open up FunnyQuotesUIForms and highlight how it works with legacy stack too
1. Edit git config repo for FunnyQuotesUIForms.yaml. Change ClientType to `wcf` (possible values are `local`, `wcf`, `asmx`, `rest`).
1. Save settings, commit and push
1. Wait at least 10 seconds and go back to the FunnyQuotesUIForms. Highlight that new provider is picked up without restart of the app

### Other things to show

* Integrated security of config server secured by OAuth. Show VCAP_Services to highlight this fact
* Config server is a Java Spring app
* Spring uses the same layered config as in .NET core. Environmental variable to set URL of repo is actually using the same config sementics but in Java 


### Things to highlight in code

1.  `.AddConfigServer(env, logFactory)` in Global.cs (legacy apps) or Program.cs (.NET core). 
1. Open up appsettings.json
    1. Explain how name of app comes from `spring:application:name`
    1. Explain how `spring:cloud:config` section determines settings when running config server locally
1. Different packages `Pivotal` vs `Steeltoe`. Difference is OAuth2 security. Pivotal package will work with non secured as well
1. Using Pivotal package will also add Cloud Foundry VCAP variables as a config source
1. Show registration and selection of DI instances based on config value in FunnyQuotesUIForms.Global.asmx.cs:

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
1. Explain using feature toggles to push disabled code into prod and using config to activate it vs branching
1. Highlight usages of it in codebase for security. Don't enable yet:
    1. Startup.cs in FunnyQuotesUICore

    ```csharp
     if (funnyquotesConfig.EnableSecurity) 
       authBuilder.AddCloudFoundryOAuth(Configuration);
    ```     
    
    1. Startup.cs in FunnyQuotesServicesOwin
    ```csharp
    if(funnyQuotesConfig.EnableSecurity)
      app.UseCloudFoundryJwtBearerAuthenticationconfig); 
    else
      app.Use<NoAuthenticationMiddleware>(); 
    ```