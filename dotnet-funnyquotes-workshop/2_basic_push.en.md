## Pushing apps and creating services
 
### Goals
1. Show how pushing .NET Framework 4.x apps and .NET Core apps work the same.
1. Target both Linux and Windows stacks.
1. Create marketplace services.

### Pushing apps
1. Push the FunnyQuotesUICore frontend from the publish folder.

    ```
    > cd FunnyQuotesUICore
    > cf push
    ```
    
    * Note the default stack is `cflinuxfs2` when omitted, and the `dotnet_core_buildpack` in the manifest.yml file when pushing .NET Core apps to Linux.

1. Push the FunnyQuotesUIForms .NET Framework 4.x frontend from the publish folder.

    ```
    > cd FunnyQuotesUIForms
    > cf push
    ```
    
    * Note the `windows2016` stack and the `hwc_buildpack` in the manifest.yml file when pushing .NET Framework 4.x apps to Windows.
    * The Hosted Web Core buildpack is essentially the kernel of IIS.

1. Access the app URL, get a few funny quotes, get some laughs. :)
1. View log output

    ```
    > cf logs FunnyQuotesUICore --recent
    ```

1. Show log tailing while pushing / starting up.

    ```
    > cf logs FunnyQuotesUICore
    ```
  
1. Optional: Press Kill button and show recovery
1. Optional: Scale the app

### Creating services

1. Execute the create-services.bat file in the scripts folder.
1. Update the manifest files in FunnyQuotesUICore and FunnyQuotesUIForms to include the services section.

   ```
   services:
   - config-server
   - eureka
   - hystrix
   ```
  
  The complete manifest for FunnyQuotesUICore should be as follows.
  
  ```
  ---
  applications:

  - name: FunnyQuotesUICore
    random-route: true
    memory: 512M
    health-check-type: http
    health-check-http-endpoint: /
    buildpack: dotnet_core_buildpack
    services:
    - config-server
    - eureka
    - hystrix
  ```

  The complete manifest for FunnyQuotesUIForms should be as follows.
  
  ```
  ---
  applications:

  - name: FunnyQuotesUIForms
    random-route: true
    memory: 512M
    health-check-type: http
    health-check-http-endpoint: /
    buildpack: hwc_buildpack
    stack: windows2016
    services:
    - config-server
    - eureka
    - hystrix
  ```
