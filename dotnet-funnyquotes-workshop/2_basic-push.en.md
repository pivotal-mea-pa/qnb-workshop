## Pushing the apps
 
#### Goals 
1. Show how pushing 4.x apps and Core apps works the same.
1. Target both Linux and Windows stacks.

#### Steps

1. Push the backend - FunnyQuotesServicesOwin.

    ```
     > cd FunnyQuotesServicesOwin
     > cf push FunnyQuotesServicesOwin
    ``` 

2.Push the .NET Core front end - FunnyQuotesUICore.

    ```
     > cd FunnyQuotesUICore
     > cf push FunnyQuotesUICore
    ``` 
    
1. Explain output while app is being pushed.
1. Access the app URL, get a few funny quotes, get some laughs. :)
1. Show log output

    ```
     > cf logs FunnyQuotesUICore --recent
    ```

1. Push the backend - FunnyQuotesLegacyService.

    ```
     > cd FunnyQuotesLegacyService
     > cf push FunnyQuotesLegacyService -s windows2016
    ```
    
1. Push the 4.x WebForms front end - FunnyQuotesUIForms.

    ```
     > cd FunnyQuotesUIForms
     > cf push FunnyQuotesUIForms -s windows2016
    ```

   * _Explain Hosted Web Core buildpack (kernel of IIS)

1. Show log tailing while pushing / starting up

    ```
     > cf logs FunnyQuotesUIForms
    ```
  
1. Optional: Press Kill button and show recovery
1. Optional: Scale the app
