## Pushing basic app
 
#### Goals 
1. Show how to pushing 4.x apps and Core apps works the same
1. Targeting different stacks

#### Steps

1. Push .NET Core front end - FunnyQuotesUICore

    ```
     > cd FunnyQuotesUICore
     > cf push FunnyQuotesUICore
    ``` 
    
1. While pushing, explain buildpack output
1. Access the app URL, get a few funny quotes, get some laughs
1. Show log output

    ```
     > cf logs FunnyQuotesUICore --recent
    ```
1. Push 4.x WebForms front end - FunnyQuotesUIForms

    ```
     > cd FunnyQuotesUIForms
     > cf push FunnyQuotesUIForms -s windows2016
    ```

   * _Explain Hosted Web Core buildpack (kernel of IIS)_

1. Show log tailing while pushing / starting up

    ```
     > cf logs FunnyQuotesUIForms
    ```
  
1. Optional: Press Kill button and show recovery
1. Optional: Scale the app