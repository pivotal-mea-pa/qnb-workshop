## Pushing apps
 
### Goals
1. Show how pushing .NET Framework 4.x apps and .NET Core apps work the same.
1. Target both Linux and Windows stacks.

**Note:** Proceed with this step if you want to create all the services at once. Otherwise, each lab has the instructions for creating each service when they are needed.

### Steps
1. Execute the `create-services.bat` file in the scripts directory.

**Note:** Wait until the services are created before proceeding.

1. Push the FunnyQuotesServicesOwin backend, and the FunnyQuotesUICore frontend.

    ```
    > cd FunnyQuotesServicesOwin
    > cf push FunnyQuotesServicesOwin
    ```
    ```
    > cd FunnyQuotesUICore
    > cf push FunnyQuotesUICore
    ```
    
    * Note the default stack when omitted is `cflinuxfs2` and the use of the `dotnet_core_buildpack` when pushing .NET Core apps to Linux.
        
1. Explain output while app is being pushed.
1. Access the app URL, get a few funny quotes, get some laughs. :)
1. View log output

    ```
    > cf logs FunnyQuotesUICore --recent
    ```

1. Push the FunnyQuotesLegacyService backend, and the FunnyQuotesUIForms frontend.

    ```
    > cd FunnyQuotesLegacyService
    > cf push FunnyQuotesLegacyService
    ```
    ```
    > cd FunnyQuotesUIForms
    > cf push FunnyQuotesUIForms
    ```

    * Observe the use of the `windows2016` stack and the `hwc_buildpack` in the manifest.yml file when pushing .NET Framework apps.
    * The Hosted Web Core buildpack is essentially the kernel of IIS.

1. Show log tailing while pushing / starting up.

    ```
    > cf logs FunnyQuotesUIForms
    ```
  
1. Optional: Press Kill button and show recovery
1. Optional: Scale the app
